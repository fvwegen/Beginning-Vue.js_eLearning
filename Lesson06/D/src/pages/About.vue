<template>
    <div>
        <div>Name:{{name}}<br>Phone:{{phone}}</div>
        <div class='toast' v-show='showError'>{{errorMessage}}</div>
    </div>
</template>


<script>
import axios from 'axios'

const getData = () => axios.post('https://example.com/', {
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "ipsum": "name"
        },
        "phone": {
            "type": "string",
            "format": "phone"
        }
    }
})

export default {
    data () {
        return {
            name: undefined,
            phone: undefined,
            showError: false,
            errorMessage: undefined
        }
    },

    beforeRouteEnter(to, from, next) {
        getData().then(x => {
            next(vm => {
                vm.name = x.data.name,
                vm.phone = x.data.phone
            })
        }).catch(err => {
            next(vm => {
                vm.showError = true
                vm.errorMessage = err.message
                setTimeout(() => { 
                    vm.showError = false
                    vm.errorMessage = undefined
                }, 1000)
            })
        })
    }
}
</script>

<style scoped>
div.toast {
    width: 15em;
    height: 1em;
    position: fixed;
    bottom: 1em;
    background-color: red;
    color: white;
    padding: 1em;
    text-align: center;
}
</style>
