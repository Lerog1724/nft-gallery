<template>
  <div class="card share">
    <footer class="card-footer">
      <div class="card-footer-item">
        <b-button
          size="is-medium"
          v-clipboard:copy="realworldFullPathShare"
          @click="toast('URL copied to clipboard')"
          class="share__button"
        >
          <b-icon
            size="is-medium"
            pack="fas"
            icon="link">
          </b-icon>
        </b-button>
      </div>
      <div class="card-footer-item">
        <b-tooltip
          type="is-light"
          class="share__tooltip"
          :triggers="['click']"
          :auto-close="['outside', 'escape']"
          :active="active"
        >
          <template v-slot:content>
            <ShareNetwork
              tag="button"
              class="button share__button is-medium"
              network="twitter"
              :url="realworldFullPath"
              :title="label"
              twitter-user="KodaDot"
            >
              <b-icon
                size="is-large"
                pack="fab"
                icon="twitter"
              >
              </b-icon>
            </ShareNetwork>
            <ShareNetwork
              tag="button"
              class="button share__button is-medium"
              network="telegram"
              :url="realworldFullPath"
              :title="label"
            >
              <b-icon
                size="is-large"
                pack="fab"
                icon="telegram"
              >
              </b-icon>
            </ShareNetwork>
            <ShareNetwork
              tag="button"
              class="button share__button is-medium"
              network="line"
              :url="realworldFullPath"
              :title="label"
            >
              <b-icon
                size="is-large"
                pack="fab"
                icon="line"
              >
              </b-icon>
            </ShareNetwork>
            <ShareNetwork
              tag="button"
              class="button share__button is-medium"
              network="facebook"
              :url="realworldFullPath"
              :title="label"
            >
              <b-icon
                size="is-large"
                pack="fab"
                icon="facebook"
              >
              </b-icon>
            </ShareNetwork>
            <b-button
              size="is-medium"
              v-clipboard:copy="iframeUri"
              @click="toast('Code copied to clipboard')"
              class="share__button"
            >
              <b-icon
                size="is-medium"
                pack="fas"
                icon="code">
              </b-icon>
            </b-button>
          </template>
          <b-button
            class="share__button"
            size="is-medium"
            @click="active = !active"
            @focusout="active = !active"
          >
            <b-icon
              size="is-large"
              pack="fas"
              icon="share-alt"
            >
            </b-icon>
          </b-button>
        </b-tooltip>
      </div>
    </footer>
  </div>
</template>

<script lang="ts" >
import { Component, Prop, Vue } from 'vue-property-decorator';
import { IFrame, emptyIframe } from '../../types';

@Component({})
export default class Sharing extends Vue {
  @Prop({ default: 'Check this cool NFT on #KusamaNetwork #KodaDot' }) label!: string;
  @Prop({ default: () => emptyIframe }) iframe!: IFrame;

  private active: boolean = false;

  get helloText() {
    return this.label;
  }

  get realworldFullPath() {
    return `${window.location.origin}${this.$route.fullPath}`;
  }

  get realworldFullPathShare() {
    return `${window.location.origin}${this.$route.fullPath}`;
  }

  get telegramUri() {
    return `tg://msg_url?url=${this.realworldFullPath}&text=${this.helloText}`;
  }

  get twitterUri() {
    return `https://twitter.com/intent/tweet?text=${this.helloText}&via=KodaDot&url=${this.realworldFullPath}`;
  }

  get linemeUri() {
    return `https://lineit.line.me/share/ui?url=${this.realworldFullPath}&text=${this.helloText}`;
  }

  get width() {
    return this.iframe.width || '480px'
  }

  get height() {
    return this.iframe.height || '840px'
  }

  get customIframeUri() {
    return this.iframe.customUrl || this.realworldFullPath
  }

  get iframeUri() {
    return `
    <iframe
      src="${this.customIframeUri}"
      title="${this.label}"
      style="width:${this.width};height:${this.height};border:none;"
    ></iframe>
    `
  }

  public toast(message: string): void {
    this.$buefy.toast.open(message);
  }
}
</script>

<style lang="scss">
  @import "@/colors";

  .share {
    &__button {
      color: $primary;
      background: transparent;
      border: none;

      &:hover,
      &:focus {
        &:not(:active) {
          box-shadow: none;
        }
      }

      & > span {
        display: flex;
        align-items: center;
      }
    }

    &__tooltip {
      .tooltip-content {
        display: flex;
        flex-direction: column;

        &:before {
          display: none;
        }

        @media screen and (min-width: 1024px) {
          flex-direction: row;
        }
      }

      &.is-light  {
        .tooltip-content {
          background-color: $white;
        }
      }
    }
  }
</style>
