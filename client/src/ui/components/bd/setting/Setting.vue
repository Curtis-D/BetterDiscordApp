/**
 * BetterDiscord Setting Component
 * Copyright (c) 2015-present Jiiks/JsSucks - https://github.com/Jiiks / https://github.com/JsSucks
 * All rights reserved.
 * https://betterdiscord.net
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
*/

<template>
    <div class="bd-form-item" :class="{'bd-form-item-changed': setting.changed, 'bd-disabled': disabled, 'bd-form-item-noheader': !setting.text, 'bd-form-item-fullwidth': setting.fullwidth}">
        <BoolSetting v-if="setting.type === 'bool'" :setting="setting" :change="change" />
        <DropdownSetting v-if="setting.type === 'dropdown'" :setting="setting" :change="change" />
        <NumberSetting v-if="setting.type === 'number'" :setting="setting" :change="change" />
        <RadioSetting v-if="setting.type === 'radio'" :setting="setting" :change="change" />
        <StringSetting v-if="setting.type === 'text' && !setting.multiline" :setting="setting" :change="change" />
        <MultilineTextSetting v-if="setting.type === 'text' && setting.multiline" :setting="setting" />
        <SliderSetting v-if="setting.type === 'slider'" :setting="setting" :change="change" />
        <ColourSetting v-if="setting.type === 'colour'" :setting="setting" :change="change" />
        <KeybindSetting v-if="setting.type === 'keybind'" :setting="setting" />
        <FileSetting v-if="setting.type === 'file'" :setting="setting" :change="change" />
        <GuildSetting v-if="setting.type === 'guild'" :setting="setting" />
        <ArraySetting v-if="setting.type === 'array'" :setting="setting" :change="change" />
        <CustomSetting v-if="setting.type === 'custom'" :setting="setting" :change="change" />
        <div class="bd-form-divider"></div>
    </div>
</template>
<script>
    // Imports
    import BoolSetting from './Bool.vue';
    import DropdownSetting from './Dropdown.vue';
    import NumberSetting from './Number.vue';
    import RadioSetting from './Radio.vue';
    import StringSetting from './String.vue';
    import MultilineTextSetting from './Multiline.vue';
    import SliderSetting from './Slider.vue';
    import ColourSetting from './Colour.vue';
    import KeybindSetting from './Keybind.vue';
    import FileSetting from './File.vue';
    import GuildSetting from './Guild.vue';
    import ArraySetting from './Array.vue';
    import CustomSetting from './Custom.vue';

    export default {
        props: [
            'setting'
        ],
        components: {
            BoolSetting,
            DropdownSetting,
            NumberSetting,
            RadioSetting,
            StringSetting,
            MultilineTextSetting,
            SliderSetting,
            ColourSetting,
            KeybindSetting,
            FileSetting,
            GuildSetting,
            ArraySetting,
            CustomSetting
        },
        computed: {
            changed() {
                return this.setting.changed || false;
            },
            disabled() {
                return this.setting.disabled || false;
            }
        },
        methods: {
            change(value) {
                if (this.disabled) return;
                this.setting.value = value;
            }
        }
    }
</script>
