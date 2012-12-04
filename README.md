Calendar.jl: Calendar time for Julia
====================================

The design is heavily influenced by Hadley Wickham's lubridate package for R.

Installation
------------

To install the package:

    julia> Pkg.update()
    julia> Pkg.add("Calendar")

Then, to load into your session:

    julia> require("Calendar")
    julia> using Calendar

Creating times
--------------

```jlcon
julia> t = now()
Dec 3, 2012 12:58:52 PM EST

julia> t = ymd_hms(2013, 5, 2, 13, 45, 7)
May 2, 2013 1:45:07 PM EDT

julia> t = ymd_hms(2013, 5, 2, 13, 45, 7, "PST")
May 2, 2013 1:45:07 PM PDT
```

String formatting & parsing
---------------------------

```jlcon
julia> t = ymd_hms(2013, 3, 10, 1, 59, 59)
Mar 10, 2013 1:59:59 AM EST

julia> s = format("yyyy-MMMM-dd HH:mm:ss V", t)
"2013-March-10 01:59:59 EST"

julia> t2 = parse("yyyy-MMMM-dd HH:mm:ss V", s)
Mar 10, 2013 1:59:59 AM EST

julia> t == t2
true
```

See [here](http://userguide.icu-project.org/formatparse/datetime) for a list of format codes.

Extracting & setting fields
---------------------------

```jlcon
julia> t
May 2, 2013 1:45:07 PM PDT

julia> month(t)
5

julia> week(t)
18

julia> am(t)
false
```

Available fields:

<table>
<tr><td>
    year(d)       </td><td>
</td></tr>
<tr><td>
    month(d)      </td><td> numbered 1-12
</td></tr>
<tr><td>
    week(d)       </td><td> week of year
</td></tr>
<tr><td>
    day(d)        </td><td> day of month
</td></tr>
<tr><td>
    dayofyear(d)  </td><td>
</td></tr>
<tr><td>
    hour(d)       </td><td> 24hr clock
</td></tr>
<tr><td>
    hour12(d)     </td><td> 12hr clock
</td></tr>
<tr><td>
    minute(d)     </td><td>
</td></tr>
<tr><td>
    second(d)     </td><td>
</td></tr>
<tr><td>
    am(d)         </td><td> is time before noon?
</td></tr>
<tr><td>
    pm(d)         </td><td> is time after noon?
</td></tr>
</table>

The two argument form lets you set individual fields:

```jlcon
julia> t2 = now()
Dec 3, 2012 3:53:08 PM EST

julia> minute!(t2, 7)       # modifies t2
Dec 3, 2012 3:07:08 PM EST

julia> year(t2, 1984)       # doesn't modify t2
Dec 3, 1984 3:07:08 PM EST
```
 
Durations
---------

```jlcon
julia> t
May 2, 2013 1:45:07 PM PDT

julia> t + months(2)
Jul 2, 2013 1:45:07 PM PDT

julia> t + days(60)
Jul 1, 2013 1:45:07 PM PDT

julia> d = years(1) + minutes(44)
1 year, 44 minutes

julia> t + d
May 2, 2014 2:29:07 PM PDT
```

Available durations: `years, months, weeks, days, hours, minutes, seconds`

Timezones
---------

```jlcon
julia> est = ymd_hms(2013, 3, 10, 1, 59, 59)
Mar 10, 2013 1:59:59 AM EST

julia> pst = tz(est, "PST")  # change timezone
Mar 9, 2013 10:59:59 PM PST

julia> est + seconds(1)      # note DST transition
Mar 10, 2013 3:00:00 AM EDT

julia> pst + seconds(1)
Mar 9, 2013 11:00:00 PM PST
```
