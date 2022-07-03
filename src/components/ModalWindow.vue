<template>
    <transition name="modal">
        <div
            class="modal-window"
            v-show="show">

            <div class="modal-window__shadow" @click.self="closeModal"></div>

            <div class="modal-window__inner" @click.self="closeModal">
                <div class="modal-window__content" ref="modalWindowContent">
                    <div class="modal-window__close-btn" @click="closeModal"></div>

                    <slot :showModalWindow="show"></slot>

                </div>
            </div>
        </div>
    </transition>
</template>

<script>

export default {
    name: "ModalWindow",

    props: ['sendStatus'],

    data: function () {
        return {
            show: false,
            browserScrollLineWidth: null,
            backToPopup: undefined
        }
    },

    methods: {
        openModal(event, backToPopup = undefined) {
            if (event) {
                event.preventDefault()
            }

            this.backToPopup = backToPopup

            this.checkWindowHeight()

            document.body.classList.add('open-modal')
            document.body.style.marginRight = this.browserScrollLineWidth
            this.show = true
        },

        closeModal() {
            this.show = false
            if (this.backToPopup) {
                window.vue.openModal(this.backToPopup)
            } else {
                document.body.classList.remove('open-modal')
                document.body.style.marginRight = '0'
            }
            this.$emit('onAfterModalClose')
        },

        getScrollLineWidth() {
            let div = document.createElement('div');

            div.style.overflowY = 'scroll';
            div.style.width = '50px';
            div.style.height = '50px';

            // мы должны вставить элемент в документ, иначе размеры будут равны 0
            document.body.append(div);
            let scrollWidth = div.offsetWidth - div.clientWidth;

            div.remove();
            return scrollWidth;
        },

        isScrollLine(a) {
            let d = document,
                b = d.body,
                e = d.documentElement,
                c = "client" + a;
            a = "scroll" + a;
            return /CSS/.test(d.compatMode) ? (e[c] < e[a]) : (b[c] < b[a])
        },

        checkWindowHeight() {
            if (this.isScrollLine('Height')) {
                this.browserScrollLineWidth = this.getScrollLineWidth() + 'px'
            } else {
                this.browserScrollLineWidth = '0'
            }
        }
    },
}
</script>

<style lang="scss">
.modal-enter-active,
.modal-leave-active {
  transition: opacity .3s
}

.modal-enter,
.modal-leave-to {
  opacity: 0
}

.modal-window {
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  z-index: 999;
  overflow: hidden;
}

.modal-window__shadow {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  top: 0;
  background: rgba(17, 17, 17, 0.5);
  opacity: 1;
}

.modal-window__inner {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  text-align: center;
  -webkit-overflow-scrolling: touch;
  overflow: auto;
  z-index: 99994;
  box-sizing: border-box;
}

.modal-window__inner:before {
  content: "";
  display: inline-block;
  vertical-align: middle;
  height: 100%;
  width: 0;
  font-size: 0;
}

.modal-window__content {
  width: auto;
  position: relative;
  display: inline-block;
  vertical-align: middle;
  margin: 0;
  border-radius: 0;
  text-align: left;
  box-sizing: border-box;

}

.modal-window__close-btn {
  position: absolute;
  right: 20px;
  top: 20px;
  width: 16px;
  height: 16px;
  cursor: pointer;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  background-image: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iOSIgaGVpZ2h0PSI5IiB2aWV3Qm94PSIwIDAgOSA5IiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxyZWN0IHg9IjcuNzc3MzQiIHdpZHRoPSIxIiBoZWlnaHQ9IjExIiB0cmFuc2Zvcm09InJvdGF0ZSg0NSA3Ljc3NzM0IDApIi8+PHJlY3QgeD0iOC40ODQzOCIgeT0iNy43NzgyIiB3aWR0aD0iMSIgaGVpZ2h0PSIxMSIgdHJhbnNmb3JtPSJyb3RhdGUoMTM1IDguNDg0MzggNy43NzgyKSIvPjwvc3ZnPg==);
  outline: none;
  transition: .2s;
  opacity: .6;
  z-index: 100;
}

.modal-window__close-btn:hover {
  opacity: 1;
}

.modal-window__footer {
  text-align: center;
}

.modal-window__close-footer-btn {
  height: 50px;
  width: 100%;
  padding: 0 50px;
  background: #0FA1DB;
  border-radius: 5px;
  border: none;
  font: 18px/50px 'Roboto';
  color: #fff;
  text-align: center;
  cursor: pointer;
  box-shadow: 1px 1px 5px 0 rgba(201, 142, 50, .0);
  transition: .2s;
}

.modal-window__close-footer-btn:hover {
  box-shadow: 1px 1px 5px 0 rgba(54, 52, 48, 0.8);
}

</style>