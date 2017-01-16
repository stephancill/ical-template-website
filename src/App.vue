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
import ical from 'ical.js'
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
            this.days.filter(day => lessons.push(...day.lessons.map(lesson => lesson.value)));
            this.fetchTemplateFrom(this.templateEndpoint, (template) => {
                // Use retrieved ics
                let icsJson = this.parseTemplate(template);
                lessons = lessons.reverse()
                const icsLessons = []
                for (var i = icsJson[2].length-1; i > 0; i--) {
                    const icsName = icsJson[2][i][1][11][3]
                    if (icsName === "CONFIRMED") {
                        // Some cases index 11 is the status and the actual event name is in 12
                        icsJson[2][i][1][12][3] = lessons[i-1]
                        // console.log(icsJson[2][i][1][12][3], lessons[i-1]);
                    } else {
                        icsJson[2][i][1][11][3] = lessons[i-1]
                        // console.log(icsJson[2][i][1][11][3], lessons[i-1]);
                    }
                }

                // for (var i = icsJson[2].length-1; i > 0; i--) {
                //     const icsName = icsJson[2][i][1][11][3]
                //     console.log("testings");
                //     if (icsName === "CONFIRMED") {
                //         // Some cases index 11 is the status and the actual event name is in 12
                //         // icsJson[2][i][1][12][3] = lessons[i-1])
                //         console.log(icsJson[2][i][1][12][3]);
                //     } else {
                //         // icsJson[2][i][1][11][3] = lessons[i-1])
                //         console.log(icsJson[2][i][1][11][3]);
                //     }
                // }
                console.log(flatten(icsJson));
                for (var i = 0; i < lessons.length; i++) {
                    // console.log(lessons[i], icsLessons[i]);
                }
            });
        },
        parseTemplate(template) {
            let jcalData = ical.parse(template)
            return jcalData
        },
        fetchTemplateFrom(endpoint, callback) {
            let ics = ""
            fetch(endpoint)
              .then(blob => blob.text())
              .then(data => { ics = data; callback(ics) });
        }

    }
}

function flatten(arr, result = []) {
  for (let i = 0, length = arr.length; i < length; i++) {
    const value = arr[i]
    if (Array.isArray(value)) {
      for (let i = 0, length = value.length; i < length; i++) {
        const value2 = value[i]
        if (Array.isArray(value2)) {
          flatten(value2, result)
        } else {
          result.push(value2)
        }
      }
    } else {
      result.push(value)
    }
  }
  return result
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
