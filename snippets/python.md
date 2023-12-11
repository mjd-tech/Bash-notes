# Python - embed in bash scripts

Source:
<http://bhfsteve.blogspot.hu/2014/07/embedding-python-in-bash-scripts.html?m=1>

Calling python from bash is easy. You simply use python's '-' argument
and send in the python code using a "heredoc". Most often, you will want
to wrap the python code in a bash function. The key thing here is that
**python is indentation sensitive**.

``` bash
#!/bin/bash

function current_datetime {
python - <<END
import datetime
print datetime.datetime.now()
END
}

# Call it
current_datetime

# Call it and capture the output
DT=$(current_datetime)
echo Current date and time: $DT
```

Notice that within the bash function, we did not indent its contents.
Any indentations here need to be what Python expects, not what makes our
bash script look pretty.

You can also pass data into your embedded python script. One way is to
use environment variables. By setting a variable on the same line that
we call python, we get an environment variable, without having to export
anything.

``` bash
#!/bin/bash

function to_upper() {
PYTHON_ARG="$1" python - << END
import os
print(os.environ['PYTHON_ARG'].upper())
END
}

echo $(to_upper fred)
# Result: FRED
```

The PYTHON_ARG variable name is arbitrary, it could be FOO, BAR, BAZ. It
is customary to make environment variables ALLCAPS, but not required.
Just make sure it doesn't clash with existing environment variables. To
check what those are run "env" at the bash prompt

    env

Another way is to pass arguments directly into the python command line.

``` bash
#!/bin/bash

function to_upper() {
python - "$1" << END
import sys
print(sys.argv[1].upper())
END
}

echo $(to_upper fred)
# Result: FRED
```

The above are contrived examples. You can "to_upper" in Bash with
parameter expansion.

Here's an example bash script that uses curl to call a REST service to
get some weather data. Then it passes the raw JSON response to an
embedded python script to interpret and format the results:

Note: needs API key to actually work
```bash
#!/bin/bash

function format_weather_data() {
PYTHON_ARG="$1" python - <<END
import os
import json

json_data = os.environ['PYTHON_ARG']
data =json.loads(json_data)
lookup = {
    '200': 'thunderstorm with light rain',
    '201': 'thunderstorm with rain',
    '202': 'thunderstorm with heavy rain',
    '210': 'light thunderstorm',
    '211': 'thunderstorm',
    '212': 'heavy thunderstorm',
    '221': 'ragged thunderstorm',
    '230': 'thunderstorm with light drizzle',
    '231': 'thunderstorm with drizzle',
    '232': 'thunderstorm with heavy drizzle',
    '300': 'light intensity drizzle',
    '301': 'drizzle',
    '302': 'heavy intensity drizzle',
    '310': 'light intensity drizzle rain',
    '311': 'drizzle rain',
    '312': 'heavy intensity drizzle rain',
    '313': 'shower rain and drizzle',
    '314': 'heavy shower rain and drizzle',
    '321': 'shower drizzle',
    '500': 'light rain',
    '501': 'moderate rain',
    '502': 'heavy intensity rain',
    '503': 'very heavy rain',
    '504': 'extreme rain',
    '511': 'freezing rain',
    '520': 'light intensity shower rain',
    '521': 'shower rain',
    '522': 'heavy intensity shower rain',
    '531': 'ragged shower rain',
    '600': 'light snow',
    '601': 'snow',
    '602': 'heavy snow',
    '611': 'sleet',
    '612': 'shower sleet',
    '615': 'light rain and snow',
    '616': 'rain and snow',
    '620': 'light shower snow',
    '621': 'shower snow',
    '622': 'heavy shower snow',
    '701': 'mist',
    '711': 'smoke',
    '721': 'haze',
    '731': 'sand, dust whirls',
    '741': 'fog',
    '751': 'sand',
    '761': 'dust',
    '762': 'volcanic ash',
    '771': 'squalls',
    '781': 'tornado',
    '800': 'clear sky',
    '801': 'few clouds',
    '802': 'scattered clouds',
    '803': 'broken clouds',
    '804': 'overcast clouds',
    '900': 'tornado',
    '901': 'tropical storm',
    '902': 'hurricane', 
    '903': 'cold',
    '904': 'hot',
    '905': 'windy',
    '906': 'hail',
    '950': 'setting',
    '951': 'calm',
    '952': 'light breeze',
    '953': 'gentle breeze',
    '954': 'moderate breeze',
    '955': 'fresh breeze',
    '956': 'strong breeze',
    '957': 'high wind, near gale',
    '958': 'gale',
    '959': 'severe gale',
    '960': 'storm',
    '961': 'violent storm',
    '962': 'hurricane',
}

print ("Current temperature: %g F" % data['main']['temp'])
print ("Today's high: %g F" % data['main']['temp_max'])
print ("Today's low: %g F" % data['main']['temp_min'])
print ("Wind speed: %g mi/hr" % data['wind']['speed'])
weather_descs = [lookup.get(str(i['id']), '*error*') for i in data['weather']]
print ("Weather: %s" % ', '.join(weather_descs))

END
}

WEATHER_URL="http://api.openweathermap.org/data/2.5/weather?q=Cincinnati,OH&units=imperial"

format_weather_data "$(curl -s $WEATHER_URL)"
```