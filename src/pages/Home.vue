<template>
  <main>
    <div class="feature-media" :class="month">
      <picture>
        <source media="(max-width: 800px)" :data-srcset="`/static/img/feature-images/${featureMedia[month]}.webp`" type="image/webp">
        <source :data-srcset="`/static/img/feature-images/${featureMedia[month]}--high.webp`" type="image/webp">
        <img
          data-sizes="auto"
          class="feature-media__media lazyload"
          :src="`/static/img/feature-images/${featureMedia[month]}--low.jpg`"
          :data-src="`/static/img/feature-images/${featureMedia[month]}.jpg`"
          :data-srcset="`/static/img/feature-images/${featureMedia[month]}.jpg 960w, /static/img/feature-images/${featureMedia[month]}--high.jpg 1920w`"
          alt=""
        >
      </picture>
      <!--<video class="feature-media__media" autoplay loop muted playsinline>
        <source src="/static/img/feature-images/Sea_Dragon.mp4" type="video/mp4">
      </video>-->
      <div class="feature-media__content">
        <div class="page-width page-padding">
          <picture v-if="anniversary">
            <source srcset="/static/img/party-quaggan.webp" type="image/webp">
            <img class="party-quaggan" src="/static/img/party-quaggan.png" alt="">
          </picture>
          <h1 class="feature-title">
            {{ anniversary ? `Celebrate ${botAge} Years of&nbsp;GW2Bot!` : 'Bring The World&nbsp;of Tyria Into&nbsp;Discord' }}
          </h1>
          <a class="feature-button button button--giant" :href="$inviteLink" target="_blank">Add To Your Server!</a>
        </div>
      </div>
    </div>
    <div class="page-width page-padding">
      <h2>GW2Bot Demo</h2>

      <div class="bot-demo">
        <video id="demo" autoplay loop muted playsinline>
          <source src="/static/img/demo.webm" type="video/webm">
          <source src="/static/img/demo.mp4" type="video/mp4">
        </video>
      </div>

      <router-link to="/commands" class="button button--giant">
        View all Commands
      </router-link>
    </div>
  </main>
</template>

<script scoped>
import "%/lazysizes/lazysizes.min.js";
import Rellax from "%/rellax/rellax.min.js";

export default {
  name: "IndexPage",
  title: "GW2Bot - A Discord Bot for Guild Wars 2",
  data() {
    return {
      botAge: (new Date()).getFullYear() - (new Date("September 9, 2017 18:18:00 GMT+00:00")).getFullYear(),
      month: ["january","february","march","april","may","june","july","august","september","october","november","december"][(new Date()).getMonth()],
      featureMedia: {
        january: "divinity-reach",
        february: "aurenes-sanctuary",
        march: "spring",
        april: "echovald-skiffs",
        may: "catacomb",
        june: "dragon-bash",
        july: "summer",
        august: "festival-of-the-four-winds",
        september: "steppe",
        october: "halloween",
        november: "autumn",
        december: "wintersday"
      }
    };
  },
  computed: {
    anniversary() {
      const today = (new Date());
      if (
        today.getMonth() === 8 &&
        today.getDate() > 8 &&
        today.getDate() < 30
      ) {
        return true;
      } else {
        return false;
      }
    }
  },
  mounted() {
    let resizeTimer;

    const rellax = new Rellax(".feature-media__media",{speed: -4});

    function responsiveRellax() {
      if(window.innerWidth < 801) {
        rellax.destroy();
      } else {
        rellax.refresh();
      }
    }

    window.addEventListener("resize",function() {
      clearTimeout(resizeTimer);
      // Debounce the resize event for performance
      resizeTimer = setTimeout(function() {
        responsiveRellax();
      }, 250);
    });

    responsiveRellax();
  }
};
</script>

<style lang="scss" scoped>
@import '~@/assets/scss/functions';
@import '~@/assets/scss/silent-classes';
@import '~@/assets/scss/colors';
@import '~@/assets/scss/settings';

.feature-media {
  position: relative;
  height: 100vh;
  height: calc(100vh - $nav-height);
  min-height: 480px;
  overflow: hidden;
}
// https://matkl.github.io/average-color/
.january {background: #5A4F3E}
.february {background: #5093D7}
.march {background: #9B9795}
.april {background: #464B30}
.may {background: #4C3A16}
.june {background: #685B50}
.july {background: #7F6857}
.august {background: #928167}
.september {background: #AB694F}
.october {background: #513927}
.november {background: #65493C}
.december {background: #878175}

.feature-media__media {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.feature-media__content {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  &:before {
    @extend %psuedo-element;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba($black,.33);
  }
}
.feature-title {
  margin: 0;
  color: $white;
  text-shadow: 0 4px  16px $black,
               0 8px  32px rgba($black,.5),
               0 16px 64px rgba($black,.25),
               0 32px 128px rgba($black,.125);
  @include media-query('gt-600') {
    font-size: $xl-font-rem;
    line-height: $xl-line-rem;
  }
}
.feature-button {
  margin-top: $baseline-rem * 2;
  box-shadow: 0 4px  16px $black,
              0 8px  32px rgba($black,.5),
              0 16px 64px rgba($black,.25),
              0 32px 128px rgba($black,.125);
}

.party-quaggan {
  display: inline-block;
  width: 100%;
  max-width: 320px;
  max-height: 394px;
  vertical-align: top;
}

.bot-demo {
  margin: $baseline-rem 0;
  video {
    display: inline-block;
    border-radius: 8px;
    width: 100%;
    max-width: 687px;
    max-height: 543px;
    vertical-align: top;
    box-shadow: $card-shadow;
    .dark-mode & {
      box-shadow: $card-shadow--dark;
    }
  }
}
</style>
