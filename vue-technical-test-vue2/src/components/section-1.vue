<template>
  <section class="section-one">
    <div class="left" ref="content">

      <!-- STEPS NAV -->
      <div class="steps">
        <span
          v-for="(slide, index) in slides"
          :key="slide.step"
          :class="{ active: activeIndex === index }"
          @click="goToSlide(index)"
        >
          {{ slide.step }}
        </span>
      </div>

      <!-- TEXT -->
      <div class="text-group">
        <h1 class="title">{{ slides[activeIndex].title }}</h1>
        <p class="desc">{{ slides[activeIndex].desc }}</p>
      </div>

    </div>

    <div class="right">
      <div class="image-wrapper">
        <img :src="sectionImage" alt="Section Visual" class="main-image" />
        <img :src="iconImage" alt="Icon" class="overlay-icon" />
      </div>

      <span class="counter">
        {{ activeIndex + 1 }} / {{ slides.length }}
      </span>
    </div>
  </section>
</template>

<script>
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import sectionImage from "@/assets/images/imgi_2_638e3f15b3ed3463ebe6038b_pexels-wendy-wei-14397945.jpg";
import iconImage from "@/assets/images/imgi_1_638e411bd0e9dd70ed4f30e8_plan.svg";
gsap.registerPlugin(ScrollTrigger);

export default {
  name: "SectionOne",
  methods:{
    goToSlide(index){
      this.activeIndex = index;
    }
  },
  data() {
    return {
      activeIndex: 1,
      sectionImage,
      iconImage,
      slides: [
        {
          title: "The sitemap of the experience",
          desc:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. A t vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea",
          step: "plan"
        },
        {
          title: "Time to paint the room walls",
          desc:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum.",
          step: "design"
        },
        {
          title: "Magic happens to build it out",
          desc:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et.",
          step: "build"
        }
      ]
    };
  },
  mounted() {
    const totalSlides = this.slides.length;

    ScrollTrigger.create({
      trigger: this.$el,
      start: "top top",
      end: () => "+=" + window.innerHeight * totalSlides,
      pin: true,
      scrub: true,
      onUpdate: (self) => {
        this.activeIndex = Math.round(
          self.progress * (totalSlides - 1)
        );
      }
    });
    this.$nextTick(() => {
      const image = this.$el.querySelector(".image-wrapper");
      if (!image) return;

      gsap.fromTo(
        image,
        { scale: 0.96 },
        {
          scale: 1,
          ease: "none",
          scrollTrigger: {
            trigger: this.$el,
            start: "top top",
            end: "bottom top",
            scrub: true
          }
        }
      );
    });
  },
  watch: {
    activeIndex() {
      if (this.$refs.content) {
        gsap.fromTo(
          this.$refs.content,
          { opacity: 0 },
          { opacity: 1, duration: 0.4, ease: "power2.out" }
        );
      }

      // Underline animation
      this.$nextTick(() => {
        const steps = this.$el.querySelectorAll(".steps span");

        steps.forEach((el, i) => {
          gsap.to(el, {
            "--underline-scale": i === this.activeIndex ? 1 : 0,
            duration: 0.4,
            ease: "power2.out"
          });
        });
      });
    }
}

};



</script>


<style lang="scss" scoped>
.section-one {
  min-height: 100vh;
  display: flex;
  justify-content: space-between;
  background: #0d1405;
  color: #c9ff4d;

  padding: 6rem 8vw;
  gap: 6vw;
}

.left {
  flex: 0 0 38%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end; 
  padding-bottom: 8vh;
}

.steps {
  margin-bottom: auto;        
  opacity: 0.35;
  font-size: 0.85rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #c9ff4d;
}

.title {
  font-size: 4.2rem;
  font-style: italic;
  font-weight: 400;
  line-height: 1.02;
  margin-bottom: 2rem;
}

.desc {
  max-width: 600px;
  font-size: 1.2rem;
  line-height: 1.9;
  opacity: 0.85;
}

.right {
  flex: 0 0 62%;
  position: relative;
}

.image-wrapper {
  position: relative;
  width: 100%;
  height: 78vh;
  min-height: 600px;
  border-radius: 40px;
  overflow: hidden;
  margin-left: auto;         
}

.main-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 40px;
}

.overlay-icon {
  position: absolute;
  top: 55%;
  left: 42%;
  width: 96px;
  height: 96px;
  transform: translate(-50%, -50%);
  pointer-events: none;
}

.image-wrapper {
  will-change: transform;
}

@media (max-width: 1024px) {
  .section-one {
    flex-direction: column;
  }

  .right {
    margin-top: 2rem;
  }
}

@media (max-width: 900px) {
  .section-one {
    flex-direction: column;
    padding: 3rem 2rem;
    gap: 3rem;
  }

  .left {
    flex: 1;
    justify-content: flex-start;
    padding-bottom: 0;
  }

  .right {
    flex: 1;
  }

  .title {
    font-size: 2.4rem;
  }

  .desc {
    font-size: 1rem;
    max-width: 100%;
  }
}

@media (max-width: 600px) {
  .image-wrapper {
    height: 42vh;
    min-height: 300px;
    border-radius: 24px;
  }

  .main-image {
    border-radius: 24px;
  }

  .overlay-icon {
    width: 64px;
    height: 64px;
    top: 50%;
    left: 50%;
  }

  .left {
    padding-bottom: 0;
  }
}

.steps {
  display: flex;
  gap: 3rem;
  margin-bottom: auto;
  color: #aff50c;
}

.steps span {
  position: relative;
  font-size: 2.5rem;
  font-weight: 500;
  letter-spacing: 0.16em;
  text-transform: lowercase;
  cursor: pointer;
  opacity: 100;
  transition: opacity 0.3s ease, color 0.3s ease;
  --underline-scale: 0;
  color: #eaff7a;
}

.steps span::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: -6px;
  width: 100%;
  height: 2px;
  background: currentColor;
  transform: scaleX(var(--underline-scale));
  transform-origin: left;
}

.steps span.active {
  opacity: 1;
  color: #e3ff6a;
}


</style>