---
title: Super Large Text Processing (Python)
tags:
  - Python
  - MySQL
abbrlink: e7e5b971
date: 2021-02-10 17:53:24
---

# Super Large Text Processing (Python)

**Recently, I got a huge data source about QQ binding mobile phone number. The txt file is an eighteen GB TXT file, and I processed the data and wrote them to database. There are mainly following parts.**

## Split File

I split the whole file into a thousand parts. (According to the last four digits of QQ, split_size = 1000)

```python
# Split the entire file into equal parts and returns a list of the positions of starting and ending byte of each segment.
def find_offsets(thread_size, file_path):
    import os
    list_start_piece_offset = list()
    # Append start offset
    list_start_piece_offset.append(0)
    file_size = os.path.getsize(file_path)
    # Each thread corresponds to a portion of the file.
    piece_size = int(file_size / thread_size)
    # print("piece_size: {}".format(piece_size))
    with open(file_path, 'rb') as f:
        for piece_number in range(1, thread_size):
            f.seek(piece_number * piece_size)
            for offset in range(0, 27):
                if f.read(1) == b'\n':
                    list_start_piece_offset.append(f.tell())
        f.close()

    # Append end offset
    list_start_piece_offset.append(file_size)
    return list_start_piece_offset
```

```python
# Here can use thread, but I didn't.
def split_big_file(size, path):
    offsets_list = find_offsets(size, path)
    # Initialize the list.
    all_piece = [''] * size

    with open(qq_data_path, 'r') as big_file:
      	# Read the file in fractions, where can use multithreading.
      	# size === len(offsets_list-1): [start, ***, end]
        for i in range(size):
          	# Each time after reading part of file, before reading next file, must claer the last result.
            for j in range(size):
                all_piece[j] = ''
            print('{}: offset({})'.format(i, offsets_list[i]))
            # read() reads the specified number of bytes from a file, or all if not given or negative.
            str_piece = big_file.read(offsets_list[i + 1] - offsets_list[i])
            list_piece = str_piece.split('\n')

            # Filter the error data (6 <= qq_len <= 10, phone_len === 11 ,2021 year)
            # Sample Format: qq----phone
            for tmp in list_piece:
                if len(tmp) > 25:
                    print('error line 1：' + str(tmp))
                    continue
                items = tmp.split("----")
                if len(items[0]) < 5 or len(items) < 2:
                    print('error line 2：' + str(tmp))
                    continue
                try:
                  	# Classify data according to the remainder.
                    m = int(int(items[0]) % size)
                    all_piece[m] += tmp + '\n'
                except BaseException as e:
                    print('error line 3：' + str(tmp))
            print('list_piece split finish')

            # Writing the data into the corresponding file.
            for j in range(size):
                with open('{}/{}.txt'.format(qq_piece_path, j), 'a') as small_file:
                    print('{}: writing'.format(j))
                    small_file.write(all_piece[j])
                    small_file.close()

        big_file.close()
```

## Create Tables in Database

Create the a thousand tables in database. (Multiplethreading)

```python
def create_table_thread(start_table, end_table):
    db = sql_init()
    cursor = db.cursor()
    counter = 0
    for i in range(start_table, end_table):
        sql = 'create table qq_' + str(i) + ' (qq varchar(12), phone varchar(11));'
        cursor.execute(sql)
        counter += 1
        # Reduce the number of commit times and increase speed.
        if counter >= 5:
            try:
                print('{} Thread Commit'.format(i))
                db.commit()
            except BaseException as e:
                db.rollback()

    db.commit()
    print('Created {} to {} table'.format(start_table, end_table))
    cursor.close()
    db.close()
```

```python
def create_table(thread_size):
    gap = int(split_size / thread_size)
    for i in range(0, thread_size):
        try:
            threading.Thread(target=create_table_thread, args=(i * gap, (i + 1) * gap)).start()
        except BaseException as e:
            print("Error: {} " + str(e))
```

## Insert Data into Database

Insert data into corresponding tables of database.

```python
# open corresponding file
def open_piece_file(num):
    with open('{}/{}.txt'.format(qq_piece_path, num), 'rb') as f:
        for item in f:
            yield str(item, encoding='utf-8')
```

```python
def insert_thread(start_piece, end_piece):
    import time
    thread_start_time = int(time.time())
    print('Start Time({} - {}): '.format(start_piece, end_piece), thread_start_time)
    # Initialize the connection of database
    db = sql_init()
    cursor = db.cursor()
    for i in range(start_piece, end_piece):
        piece_start_time = int(time.time())
        commit = 0
        counter = 0
        items = open_piece_file(i)
        sql = "insert into qq_{} (qq, phone) values".format(i)

        for item in items:
            item = item.replace('\n', '').replace('\r', '').replace('\r\n', '')
            [qq, phone] = item.split('----')
            # Multiple data insert of SQL, increase speed.
            sql += " ({}, {}),".format(qq, phone)
            counter += 1
            # Insert counter_limit of data at a time
            if counter >= counter_limit:
                try:
                    cursor.execute(sql)
                except BaseException as e:
                    print('execute error: {}'.format(e))
                    db.rollback()
                piece_end_time = int(time.time())
                # piece_time_limit executes a commit.
                if piece_end_time - piece_start_time >= piece_time_limit:
                    print('{} piece {} commit: {} items'.format(i, commit, counter))
                    try:
                        db.commit()
                    except BaseException as e:
                        print('commit error: {}'.format(e))
                    commit += 1
                    counter = 0
                    piece_start_time = int(time.time())
		# Commit the rest of SQL
    try:
        db.commit()
    except BaseException as e:
        print('commit error: {}'.format(e))
    cursor.close()
    db.close()
    thread_elapsed_time = thread_start_time - int(time.time())
    print('Elapsed Time({} - {}): {}'.format(start_piece, end_piece, thread_elapsed_time))
```

```python
def insert_data(thread_size):
    gap = int(split_size / thread_size)
    for i in range(0, thread_size):
        try:
            threading.Thread(target=insert_thread, args=(i*gap, (i+1)*gap)).start()
        except BaseException as e:
            print("Error: {} " + str(e))
```

## Reference

[Github Project](https://github.com/SmaIIstars/InformationQuery)

[超大 TXT 文本读取记录](https://myelf.club/index.php/archives/283/)
