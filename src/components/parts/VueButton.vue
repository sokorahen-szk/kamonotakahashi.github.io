<template>
    <div>
        <button class="btn" :class="addClass" @click="clickEvent">
            <slot></slot>
        </button>
    </div>
</template>

<style lang="scss" scoped>

</style>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator';

@Component
export default class VueButton extends Vue {
    @Prop({ required: false, default: 'light' }) type!: string;
    @Prop({ required: false, default: '100' }) width!: number;
    @Prop({ required: false, default: '100' }) height!: number;

    get addClass() :string[] {
        return [this.type, `w-${this.width}`, `h-${this.height}`];
    }

    private clickEvent(): void {
        this.$emit("event", true);
    }

}
</script>