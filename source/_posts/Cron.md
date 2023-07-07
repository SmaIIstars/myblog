---
title: Cron
abbrlink: b7ffafc6
date: 2021-08-18 21:53:09
tags: cron
---

# Cron

**Cron is a timed execution tool under Unix-like, which can run jobs without manual intervention.**

## Format

| Field        | Required | Range     | Special Character       | Remark                                                                                                                            |
| :----------- | :------- | :-------- | :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| Second       | True     | 0-59      | `*` `,` `-`             | The Standard implementation does not support this field                                                                           |
| Minute       | True     | 0-59      | `*` `,` `-`             |                                                                                                                                   |
| Hours        | True     | 0-23      | `*` `,` `-`             |                                                                                                                                   |
| Day of month | True     | 1-31      | `*` `,` `-` `?` `L` `W` | `?` `L` `W` Only some software implements                                                                                         |
| Month        | True     | 1-12      | `*` `,` `-`             |                                                                                                                                   |
| Day of week  | True     | 0-7       | `*` `,` `-` `?` `L` `#` | `?` `L` `#` Only some software implements<br/>(Linux and Spring value range are 0-7)<br/>(Quartz value range is 1-7, 1 is Sunday) |
| Year         | False    | 1970-2099 | `*` `,` `-`             | The Standard implementation does not support this field                                                                           |

> ### Character
>
> - `*` means all values.
> - `,` means partition.
> - `-` indicates closed interval continuous.
> - `L` is the 'Last'. When used in 'Day of week' field, it's possible to specify a structure for month, such as "Last Friday"(5L). In the 'Month' field, you can specify the last day of current month.
> - `W` is 'Day of month', it can specify the working day closest to the target date, but it cannot span the current month. (Eg. '15W': targetDate = 15th === Saturday ? 14th : 15th === Sunday && 16th)
> - `#` is used in 'Day of week' field and must be followed by a number between 1 and 5. For example, 5#3 means the third Friday every month.
> - `?` means blank or will be replaced by the time of process started.

## Custom Cron Expression Web Site

### Supported Format

```basic
*    *    *    *    *    *
┬    ┬    ┬    ┬    ┬    ┬
│    │    │    │    │    |
│    │    │    │    │    └ day of week (0 - 7, 1L - 7L) (0 or 7 is Sun)
│    │    │    │    └───── month (1 - 12)
│    │    │    └────────── day of month (1 - 31, L)
│    │    └─────────────── hour (0 - 23)
│    └──────────────────── minute (0 - 59)
└───────────────────────── second (0 - 59, optional)
```

### Cron Expression Web Site

<iframe  
 height=850 
 width=90% 
 src="http://cron.smallstars.top"  
 frameborder=0>
 </iframe>

## References

- [Cron Quartz 定时器调试工具](https://tool.ityuan.com/cron)
- [Linux crontab 命令](https://www.runoob.com/linux/linux-comm-crontab.html)
- [cron 表达式的用法](https://www.cnblogs.com/dubhlinn/p/10740838.html)
- [cron-parser](https://www.npmjs.com/package/cron-parser)
- [Cron Expression](http://cron.smallstars.top)
