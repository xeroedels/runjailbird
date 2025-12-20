<template>
  <section class="section-two">
    <div class="inner">
      <span class="count">1—3</span>

      <h2 class="word">
        {{ currentword }}
      </h2>
      <button class="cta">Learn More</button>
    </div>
  </section>
</template>

<script>
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import image from "@/assets/images/imgi_2_638e3f15b3ed3463ebe6038b_pexels-wendy-wei-14397945.jpg";

gsap.registerPlugin(ScrollTrigger);

export default {
  name: "SectionTwo",
  data() {
    return {
      image,
      words: ["plan", "design", "build"],
      currentword: "plan"
    };
  },
  methods: {
    changeWord(word) {
        const el = this.$el.querySelector(".word");

        gsap.fromTo(
        el,
        { opacity: 0, y: 30 },
        { opacity: 1, y: 0, duration: 0.6, ease: "power2.out" }
        );

        this.currentWord = word;
    }
  },

  mounted() {
    const totalScroll = window.innerHeight * 3.5;
    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: this.$el,
        start: "top top",
        end: () => "+=" + window.innerHeight * 3.5,
        pin: true,
        scrub: true
      }
    });

    tl.fromTo(
      ".image-mask img",
      { scale: 0.6 },
      { scale: 1.4, ease: "none" }
    );
    this.words.forEach((word, i) => {
        ScrollTrigger.create({
            trigger: this.$el,
            start: () => `${(totalScroll / this.words.length) * i}px top`,
            end: () => `${(totalScroll / this.words.length) * (i + 1)}px top`,
            onEnter: () => this.changeWord(word),
            onEnterBack: () => this.changeWord(word)
        });
    });
  }
};
</script>

<style lang="scss" scoped>
.section-two {
  min-height: 100vh;
  background: #0d1405;
  display: flex;
  align-items: center;
  justify-content: center;
}

.inner {
  position: relative;
  text-align: center;
  color: #c9ff4d;
}

.count {
  display: block;
  font-size: 0.85rem;
  letter-spacing: 0.2em;
  margin-bottom: 2rem;
  opacity: 0.7;
}

.word {
  position: relative;
  font-size: 12vw;
  font-style: italic;
  font-weight: 400;
  line-height: 1;
  color: #c9ff4d;
  z-index: 3;
  pointer-events: none;
  text-align: center;
  text-shadow:
    0 0 20px rgba(201, 255, 77, 0.25),
    0 0 40px rgba(201, 255, 77, 0.15);
}

.image-mask {
  position: fixed;
  inset: -20%;
  z-index: -1;
  overflow: hidden;
  top: 50%;
  left: 50%;
  width: 120vh;
  height: 120vh;
  transform: translate(-50%, -50%);
  pointer-events: none;
}

.image-mask img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transform-origin: center;
  will-change: transform;
}

.cta {
  background: transparent;
  border: 1px solid #c9ff4d;
  color: #c9ff4d;
  padding: 0.75rem 2rem;
  border-radius: 999px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background 0.3s ease;
}

.cta:hover {
  background: rgba(201, 255, 77, 0.1);
}
</style>
