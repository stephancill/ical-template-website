<template>
    <div id="app">
        <ul id="day">
            <div id="container">
                <br>
                <li v-for="day in days">
                    <div style="float: right;">
                        <label for="">{{day.name}}</label>
                        <li v-for="lesson in day.lessons">
                            <input v-model="lesson.value" type="text" name="" :placeholder="lesson.name">
                            <!-- <input  name="" name=""> -->
                        </li>
                    </div>
                </li>
                <button @click="getSubjects" type="button" name="button">Test</button>
            </div>
        </ul>
    </div>
</template>

<script>
import icalendar from 'icalendar'
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
                lessons = lessons.reverse()
                let calendar = this.parseTemplate(template);
                console.log(lessons);
                for (var i = calendar.events().length-1; i > -1; i--) {
                    calendar.events()[i].setSummary(lessons[i])
                    console.log(calendar.events()[i].properties.SUMMARY[1].value, lessons[i]);
                }
                // console.log(calendar.toString());
                var blob = new Blob([calendar.toString()], {type: "text/plain;charset=utf-8"});
                FileSaver.saveAs(blob, "2017_Q1_Timetable.ics");
            });
        },
        parseTemplate(template) {
            var ical = icalendar.parse_calendar(template);
            return ical
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
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
#container {
    width: 100px
}
</style>
