<template>
    <div class="portfolio">

        <div class="portfolio__wrap">

            <template v-if="getFirebase">

            <p>
                <VueButton type="dark" @event="prevItem()">＜</VueButton>
            </p>

            <template v-for="(item, index) in getFirebase">
            <template v-if="index < getContentCardLength() ">
            <VueSiteBoxCard
                height="260"
                :path="item.path"
                :godCount="item.goodCount"
                :badCount="item.badCount"
                :key="index"
                :published="item.publish"
            >
                <template v-slot:card-title>
                    {{item.siteName}}
                </template>
                <template v-slot:card-image>
                    <img
                        :src="item.image.url"
                        :width="`${item.image.width}%`"
                        :height="item.image.height"
                    />
                </template>
                <template v-slot:card-action>
                    <dl>
                        <dd>
                            <time>{{item.createdDate}}</time>
                        </dd>
                        <dd>
                            <i class="fas fa-thumbs-up fa-thumbs-good"></i>
                            <i class="fas fa-thumbs-down fa-thumbs-bad"></i>
                        </dd>
                    </dl>
                </template>
            </VueSiteBoxCard>

            </template>
            </template>

            <p>
                <VueButton type="dark" @event="nextItem()">＞</VueButton>
            </p>

            </template>

        </div>
    </div>
</template>

<style lang="scss" scoped>
</style>

<script lang="ts">
import { Vue, Component, Mixins, Watch } from 'vue-property-decorator';
import StoreMixin from '@/store/global';

import { ProfilePortfolio } from '@/types/portfolio';
import { Client } from '@/types/client';

import VueSiteBoxCard from '@/components/parts/VueSiteBoxCard.vue';
import VueButton from '@/components/parts/VueButton.vue';

@Component({
    components: {
        VueSiteBoxCard,
        VueButton
    }
})
export default class VieHomePortfolio extends Mixins(StoreMixin) {

    /* data */
    private portfolio :ProfilePortfolio[] = [];

    /* Watch */
    @Watch('_firebase', { deep: true })
    private onFirebaseResponseChange(_portfolio: ProfilePortfolio[]) {
        this.portfolio = _portfolio;
    }

    /* Methods */
    private getContentCardLength() :number {
        if(1000 < this._client.window_width) {
            return 3;
        } else if( 700 < this._client.window_width) {
            return 2;
        }
        return 1;
    }
    private prevItem() :void {
        this.portfolio.push(this.portfolio.shift()!);
    }
    private nextItem() :void {
        this.portfolio.unshift(this.portfolio.pop()!);
    }

    /* Computed */
    private get getFirebase() :ProfilePortfolio[] {
        return this.portfolio;
    }

    private get clientWidth():number {
        return this._client.window_width;
    };

    /* Created */
    created() {
        // firebase ロード
        this.setFirebase("portfolio");
        this.$nextTick( () => {
            this.portfolio = this._firebase;
        });
    }

}
</script>