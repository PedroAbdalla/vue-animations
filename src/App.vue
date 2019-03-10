<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model="alterAnimaTion" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <hr>
                <button class="btn btn-primary" @click="show = !show">SHow Alert</button>
                <br><br>
                <transition :name="alterAnimaTion">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <transition :name="alterAnimaTion" type="animation">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <transition 
                    appear
                    enter-active-class="animated bounce"
                    leave-active-class="animated shake">
                    <div class="alert alert-info" v-if="show">This is some info</div>
                </transition>
                <transition :name="alterAnimaTion" mode="out-in">
                    <div class="alert alert-info" v-if="show" key="info">This is some info</div>
                    <div class="alert alert-warning" v-else key="warning">This is some warning</div>
                </transition>
                <button class="btn btn-primary" @click="load = !load">Load or remove</button>
                <br><br>
                <transition
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afeterEnter"
                    @enter-cancelled="enterCancelled"

                    @before-leave="beforeLeave"
                    @leave="leave"
                    @after-leave="afeterLeave"
                    @leave-cancelled="LeaveCancelled"
                    ><!-- Comment -->
                    <!-- nao procura as classes padroes do transtion fade-enter slide-enter,... -->
                    :css="false">
                    <div style="width:300px; height:100px; background-color: lightgreen;" v-if="load"></div>
                </transition>
                <hr>
                <br>
                <button 
                    class="btn btn-primary"
                    @click="selectedComponent == 'app-succes-alert' ? selectedComponent = 'app-danger-alert' : selectedComponent = 'app-succes-alert'">Toogle Components</button>
                <transition name="fade" mode="out-in">
                    <component :is="selectedComponent"></component>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    import DangerAlert from './DangerAlert.vue'
    import SuccesAlert from './SuccesAlert.vue'

    export default {
        data() {
            return {
                show: false,
                load: true,
                alterAnimaTion: 'fade',
                elementWidth: 100,
                selectedComponent: 'app-succes-alert'
            }
        },
        methods: {
            beforeEnter(el) {
                console.log('beforeEnter')
                this.elementWidth = 100
                el.style.widt = this.elementWidth + 'px'
            },
            enter(el, done) {
                console.log('enter')
                let round = 1
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px'
                    round++
                    if(round>20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20)
            },
            afeterEnter(el) {
                console.log('afeterEnter')
            },
            enterCancelled(el) {
                console.log('enterCancelled')
            },
            beforeLeave(el) {
                console.log('beforeLeave')
                this.elementWidth = 300
                el.style.width = this.elementWidt + 'px'
            },
            leave(el, done) {
                console.log('leave')
                let round = 1
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - round * 10) + 'px'
                    round++
                    if(round>20) {
                        clearInterval(interval)
                        done()
                    }
                }, 20)
            },
            afeterLeave(el) {
                console.log('afeterLeave')
            },
            LeaveCancelled(el) {
                console.log('LeaveCancelled')
            },
        },
        components: {
            appDangerAlert: DangerAlert,
            appSuccesAlert: SuccesAlert        }
    }
</script>
<style>
    .fade-enter {
        opacity: 0;
    }
    .fade-enter-active {
        transition: opacity 1s;
    }
    .fade-leave {

    }
    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }
    .slide-enter {
        opacity: 0;
        
    }
    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }
    .slide-leave {

    }
    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
    }
    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }
    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
</style>
