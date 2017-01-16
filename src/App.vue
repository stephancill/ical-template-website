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
import ical from 'ical-generator'
export default {
    name: 'app',
    data () {
        return {
            days:
                [
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
        getSubjects(event) {
            const lessons = []
            this.days.filter(day => lessons.push(...day.lessons.map(lesson => lesson.name)));
            console.log(lessons);
        },

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
