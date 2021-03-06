/**
 * BetterDiscord Setting Keybind Component
 * Copyright (c) 2015-present Jiiks/JsSucks - https://github.com/Jiiks / https://github.com/JsSucks
 * All rights reserved.
 * https://betterdiscord.net
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
*/

<template>
    <div class="bd-form-keybind">
        <div class="bd-form-keybind-details">
            <div class="bd-title">
                <h3>{{ setting.text }}</h3>
            </div>
            <div class="bd-hint">{{ setting.hint }}</div>
        </div>
        <div class="bd-keybind" :class="{'bd-active': active, 'bd-disabled': setting.disabled, 'bd-keybind-unset': !setting.value}">
            <div class="bd-keybind-selected">{{ selected || 'No Keybind Set' }}</div>
            <button class="bd-button" v-tooltip="`Click to record a new keybind sequence${setting.value ? ' (shift + click to delete the sequence)' : ''}`" @click="$event.shiftKey ? deleteKeybind() : toggleActive(); $event.target.blur()">{{ active ? 'Stop Recording' : setting.value ? 'Edit Keybind' : 'Record Keybind' }}</button>
        </div>
    </div>
</template>

<script>
    import { shell } from 'electron';
    import { ClientIPC } from 'common';
    import Combokeys from 'combokeys';
    import CombokeysRecord from 'combokeys/plugins/record';

    const combokeys = new Combokeys(document);
    CombokeysRecord(combokeys);

    const process = window.require('process');
    const modifierKey = process.platform === 'darwin' ? 'meta' : 'ctrl';

    export default {
        props: ['setting'],
        data() {
            return {
                active: false
            };
        },
        computed: {
            selected() {
                return this.getDisplayString(this.setting.value);
            }
        },
        watch: {
            active(active) {
                if (active) combokeys.record(this.recorded);
            }
        },
        methods: {
            toggleActive() {
                if (this.setting.disabled) return;
                this.active = !this.active;
            },
            deleteKeybind() {
                this.setting.value = '';
            },
            recorded(sequence) {
                if (!this.active) return;
                this.active = false;
                this.recordingValue = undefined;
                this.setting.value = sequence.join(' ');
                console.log('keypress', sequence);
            },
            getDisplayString(value) {
                if (!value) return;
                return value.split(' ').map(pattern => {
                    return pattern.toUpperCase().replace(/\+/g, ' + ').replace(/mod/gi, modifierKey).replace(/meta/gi, 'Cmd').replace(/ctrl/gi, 'Ctrl').replace(/alt/gi, 'Alt').replace(/shift/gi, 'Shift');
                }).join(', ');
            }
        }
    }
</script>
