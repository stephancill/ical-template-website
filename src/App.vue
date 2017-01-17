<template>
    <div id="app">
        <h4>Disclaimer: Does not currently support iOS</h4>
        <h3>Instructions:</h3>
        <p>Enter subject names, press the "Generate and download" button at the bottom of the page, <a href="https://docs.google.com/document/d/1FOt5v8nEpa0tXRXqlaJvBuNGLsiRC5EckL3zvCWZLj8">import the downloaded file into Google Calendar</a></p>
        <form class="" onsubmit="return false;" method="post">
            <div id="container">
                <div style="margin-left:auto;margin-right:auto;">
                    <ul id="day">
                        <br>
                        <li v-for="day in days">
                            <div style="margin-right: auto;">
                                <label for="">{{day.name}}</label>
                                <li v-for="lesson in day.lessons">
                                    <input v-model="lesson.value" type="text" name="" :placeholder="lesson.name">
                                    <!-- <input  name="" name=""> -->
                                </li>
                            </div>
                        </li>


                    </ul>
                </div>
            </div>
            <button @click="getSubjects" type="submit" name="button">Generate and download</button>
        </form>


    </div>
</template>

<script>
// import icalendar from 'icalendar'
import FileSaver from 'file-saver'

export default {
    name: 'app',
    data () {
        return {
            templateEndpoint: 'https://raw.githubusercontent.com/stephancill/ical-template-website/master/src/assets/2017_Blank_Timetable.ics',
            days: [
                {
                    name: 'Monday',
                    lessons: [
                        {
                            name: 'L1',
                            value: ''
                        }, {
                            name: 'L2',
                            value: ''
                        }, {
                            name: 'Utility',
                            value: ''
                        }, {
                            name: 'L3',
                            value: ''
                        }, {
                            name: 'L4',
                            value: ''
                        }, {
                            name: 'L5',
                            value: ''
                        }, {
                            name: 'L6',
                            value: ''
                        }, {
                            name: 'L7',
                            value: ''
                        }
                    ]
                }, {
                    name: 'Tuesday',
                    lessons: [
                        {
                            name: 'L1',
                            value: ''
                        }, {
                            name: 'L2',
                            value: ''
                        }, {
                            name: 'Utility',
                            value: ''
                        }, {
                            name: 'L3',
                            value: ''
                        }, {
                            name: 'L4',
                            value: ''
                        }, {
                            name: 'L5',
                            value: ''
                        }, {
                            name: 'L6',
                            value: ''
                        }, {
                            name: 'L7',
                            value: ''
                        }
                    ]
                }, {
                    name: 'Wednesday',
                    lessons: [
                        {
                            name: 'L1',
                            value: ''
                        }, {
                            name: 'L2',
                            value: ''
                        }, {
                            name: 'Utility',
                            value: ''
                        }, {
                            name: 'L3',
                            value: ''
                        }, {
                            name: 'L4',
                            value: ''
                        }, {
                            name: 'L5',
                            value: ''
                        }, {
                            name: 'L6',
                            value: ''
                        }, {
                            name: 'L7',
                            value: ''
                        }
                    ]
                }, {
                    name: 'Thursday',
                    lessons: [
                        {
                            name: 'L1',
                            value: ''
                        }, {
                            name: 'L2',
                            value: ''
                        }, {
                            name: 'Utility',
                            value: ''
                        }, {
                            name: 'L3',
                            value: ''
                        }, {
                            name: 'L4',
                            value: ''
                        }, {
                            name: 'L5',
                            value: ''
                        }, {
                            name: 'L6',
                            value: ''
                        }, {
                            name: 'L7',
                            value: ''
                        }
                    ]
                }, {
                    name: 'Friday',
                    lessons: [
                        {
                            name: 'L1',
                            value: ''
                        }, {
                            name: 'L2',
                            value: ''
                        }, {
                            name: 'L3',
                            value: ''
                        }, {
                            name: 'L4',
                            value: ''
                        }, {
                            name: 'L5',
                            value: ''
                        }, {
                            name: 'L6',
                            value: ''
                        }, {
                            name: 'Assembly',
                            value: ''
                        }
                    ]
                }
            ]
        }
    },
    methods: {
        getSubjects() {
            let lessons = []
            this.days.filter(day => lessons.push(...day.lessons.map(lesson => {
                if (lesson.value != '') {
                    return lesson.value
                } else {
                    return lesson.name
                }
            })));
            this.fetchTemplateFrom(this.templateEndpoint, (template) => {
                // Use retrieved ics
                // lessons = lessons.reverse()
                let calendar = this.parseTemplate(template).split("\n");
                // console.log(lessons);
                // console.log(calendar.events());
                let counter = lessons.length-1
                for (var i = calendar.length-1; i > -1; i--) {
                    // calendar.events()[i].setSummary(lessons[i])
                    if (calendar[i].slice(0, 7) === "SUMMARY"){
                        calendar[i] = `SUMMARY:${lessons[counter]}`
                        // console.log(calendar[i], lessons[counter]);
                        counter -= 1
                    }
                    // console.log(calendar[i]);
                    // console.log(calendar.events()[i].properties.SUMMARY[1].value, calendar.events()[i].properties.DTSTART[0].value);
                    // console.log(calendar.events()[i].toString());
                }
                // console.log(calendar.toString());
                var blob = new Blob([calendar.join("\n")], {type: "text/plain;charset=utf-8"});
                FileSaver.saveAs(blob, "2017_Q1_Timetable.ics");
            });
        },
        parseTemplate(template) {
            // console.log(template);
            // var ical = icalendar.parse_calendar(template);
            return template
        },
        fetchTemplateFrom(endpoint, callback) {
            let ics = ""
            fetch(endpoint)
              .then(blob => blob.text())
              .then(data => { ics = data; callback(ics) });
        }

    }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  /*padding: 0;*/
}

li {
  /*display: inline-block;*/
  /*margin: 0 10px;*/
}

a {
  color: #42b983;
}
#container {
    width: 100px
}


</style>
