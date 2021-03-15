<template>
    <default-field :field="field" :errors="errors">
        <template slot="field">
            <date-picker
                    :id="field.name"
                    type="date"
                    :class="errorClasses"
                    :clearable="true"
                    :color="color"
                    :locale="locale"
                    min="378/10/11"
                    max="1450/12/29"
                    :placeholder="placeholder"
                    inputClass="w-full form-control form-input form-input-bordered date-ltr"
                    :format="format"
                    :initial-value="persianDate"
                    v-model="value"
            >
                <svg slot="clear-btn" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 352 512" style="height: 16px; margin-top: 10px;">
                    <path
                        fill="currentColor"
                        d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"></path>
                </svg>
            </date-picker>
        </template>
    </default-field>
</template>

<script>
    import VuePersianDatetimePicker from 'vue-persian-datetime-picker'
    import jMoment from "moment-jalaali"
    import {FormField, HandlesValidationErrors} from 'laravel-nova'

    export default {
        mixins: [FormField, HandlesValidationErrors],
        props: ['resourceName', 'resourceId', 'field'],
        computed: {
            format() {
                return this.field.format || 'jYYYY/jMM/jDD';
            },
            persianDate() {
                if (!this.field.value) {
                    return '';
                } else {
                    return jMoment(this.field.value).format(this.format);
                }
            },
            placeholder() {
                return this.field.placeholder || jMoment().format(this.format)
            },
            altDateValue() {
                return this.value ? jMoment(this.value, this.format).format('YYYY-MM-DD') : '';
            },
            color() {
                return this.field.color || 'rgb(30, 136, 229)';
            },
            locale() {
                return this.field.locale || 'fa,en';
            }
        },
        methods: {
            setInitialValue() {
                this.value = this.persianDate;
            },
            fill(formData) {
                formData.append(this.field.attribute, this.altDateValue || '');
            },
        },
        components: {
            datePicker: VuePersianDatetimePicker
        }
    }
</script>

<style>
    .date-ltr {
        direction: ltr !important;
        text-align: right;
    }
</style>