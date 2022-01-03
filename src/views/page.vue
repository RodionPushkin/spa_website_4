<template>
  <div class="page">
    <div class="slider">
      <div class="slider__wrapp" ref="slider">
      </div>
      <ul class="slider__buttons" ref="buttons">
      </ul>
    </div>
  </div>
</template>
<script>
import gsap from 'gsap'
export default {
  data(){
    return{
      settings:{
        autoslide: false,
        mobile: false,
      },
      currentIndex: 0,
      slides:[
        {
          url: "https://picsum.photos/1280/720",
        },
        {
          url: "https://picsum.photos/1280/720",
        },
        {
          url: "https://picsum.photos/1280/720",
        },
        {
          url: "https://picsum.photos/1280/720",
        },
        {
          url: "https://picsum.photos/1280/720",
        },
      ]
    }
  },
  mounted(){
    this.setup()
  },
  methods:{
    autoslide(){

    },
    changeSlide(index){
      let slides = this.$refs.slider.querySelectorAll('.slide')
      for (let i = 0; i < slides.length; i++) {
        slides[i].style.marginLeft = null
        if(index == undefined && this.currentIndex+1 == i){
          if(i != 0){
            slides[i-1].style.marginLeft = "-100vw"
          }
          slides[i].style.marginLeft = "0vw"
          if(i != slides.length){
            slides[i+1].style.marginLeft = "-100vw"
          }
          this.currentIndex++
        }
        else if(index == i){
          if(i != 0){
            slides[i-1].style.marginLeft = "-100vw"
          }
          slides[i].style.marginLeft = "0vw"
          if(i != slides.length){
            slides[i+1].style.marginLeft = "-100vw"
          }
          this.currentIndex = i
        }
        break
      }
    },
    setup(){
      for (let i = 0; i < this.slides.length; i++) {
        let image = `<img class="slide__image" draggable="false" src="${this.slides[i].url}" >`
        this.slides[i].index = i
        this.$refs.slider.innerHTML += `<div class="slide">${image}</div>`
        this.$refs.buttons.innerHTML += `<li class="slider__button" data-index="${i}"><div></div></li>`
      }
      let buttons = this.$refs.buttons.querySelectorAll('.slider__button')
      buttons[this.currentIndex].classList.add('slider__button-active')
      this.changeSlide(this.currentIndex) 
      for (let i = 0; i < buttons.length; i++) {
        buttons[i].addEventListener('click', (e)=>{
          if(!e.target.classList.contains('slider__button-active')){
            for (let i = 0; i < buttons.length; i++) {
              if(buttons[i].classList.contains('slider__button-active')){
                buttons[i].classList.remove('slider__button-active')
              }
            }
            e.target.classList.add('slider__button-active')
            this.changeSlide(e.target.getAttribute('data-index'))
          }
        })
      }
    },
  }
}
</script>
<style lang="scss">
  .slider{
    height: 100vh;
    width: 100vw;
    position: relative;
  }
  .slider__wrapp{
    display: flex;
    position: relative;
    .slide{
      width: 100vw;
      height: 100vh;
      position: absolute;
      transition: 1s;
      .slide__image{
        width: 100vw;
        height: 100vh;
        object-fit: cover;
        cursor: auto;
      }
    }
  }
  .slider__buttons{
    display: flex;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 56px;
    .slider__button{
      padding: 4px;
      cursor: pointer;
    }
    .slider__button div{
      height: 8px;
      width: 8px;
      background: #000;
      border-radius: 8px;
      transition: .1s linear;
      pointer-events: none;
    }
    .slider__button-active div{
      width: 40px;
    }
  }
</style>
