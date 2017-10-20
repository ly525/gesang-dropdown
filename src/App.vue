<template>
  <div id="app">
    <gesang-dropdown
        prop="city"
        title-suffix="new prop"
        :init-checked-list="initCheckedList"
        :full-list="fullList"
        :full-key-value-map="fullMap"
        :on-checked="handleChecked">
    </gesang-dropdown>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
import gesangDropdown from './dropdown.vue'

export default {
    name: 'app',
    data() {
        return {
            initCheckedList: [1, 2, 3],
            fullList: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14, 15, 16, 17],
            fullMap: {
                1: 'a',
                2: 'abc',
                3: 'abcd',
                4: 'abcd',
                5: 'abcde',
                6: 'abcdef',
                7: 'abcdefg',
                8: 'abcdefgh',
                9: 'abcdefghi',
                10: 'abcdefghik',
                11: 'abcdefghikl',
                12: 'abcdefghiklm',
                13: 'abcdefghiklmn',
                14: 'abcdefghiklmno',
                15: 'abcdefghiklmnop',
                16: 'abcdefghiklmnopq',
                17: 'longlonglonglonglonglonglonglonglonglonglong',
            },
        }
    },
    methods: {
        handleChecked(items, prop) {
            console.log('App.vue')
            console.log(items)
            console.log(prop)
        },
    },
    components: {
        gesangDropdown,
    },
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
</style>
