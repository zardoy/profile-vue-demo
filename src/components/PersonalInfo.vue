<script setup lang="ts">
import { Form } from 'vee-validate'
import { ref } from 'vue'
import Input from './Input.vue'
import { spokenLanguages } from '../staticData'
import SubmitButton from './SubmitButton.vue'

const selectedSpokenLanguages = ref<string[]>([])

const validationSchema = {
    fullName(value: string | undefined) {
        value ??= ''
        if (value.length < 3) {
            return 'Full name must be at least 3 characters'
        }

        return true
    },
    birthDate(value: string | undefined) {
        if (!value) return true
        const date = new Date(value)
        if (date.toString() === 'Invalid Date') {
            return 'Birth date is invalid'
        }
        if (date > new Date()) {
            return 'Birth date cannot be in the future'
        }

        return true
    },
    email(value: string | undefined) {
        if (!value) {
            return 'Email is required'
        }
        if (value.length < 4) {
            return 'Email must be at least 3 characters'
        }
        if (!value.includes('@')) {
            return 'Email is invalid'
        }

        return true
    },
    city(value: string | undefined) {
        value ??= ''
        if (value.length === 0) return true
        if (value.length < 3) {
            return 'City must be at least 3 characters'
        }

        return true
    },
    phone(value: string | undefined) {
        if (!value) return true
        // validate phone number with regex
        if (!value.match(/^\d{3}-\d{3}-\d{2}-\d{2}$/)) {
            return 'Phone number is invalid'
        }

        return true
    },
}

const changeSelect = ev => {
    selectedSpokenLanguages.value.push([...ev.target.options].find(o => o.value === ev.target.value).textContent)
    ev.target.value = ''
}

const removeSelectedItem = value => {
    selectedSpokenLanguages.value.splice(selectedSpokenLanguages.value.indexOf(value), 1)
}

const submit = values => {
    console.log('submit', values)
}

const onlyNumbersKeydownHandler = (ev: KeyboardEvent) => {
    const { key } = ev
    if (ev.ctrlKey || ev.metaKey) return
    // chromium autofill fires it without keyboard interface impl (no key)
    if (key?.length === 1 && !key.match(/\d/)) {
        ev.preventDefault()
    }
}

const valueWasEmpty = ref(true)

const phoneNumberInputChange = (ev: Event) => {
    const target = ev.target as HTMLInputElement
    // input event bubbles
    if (target !== ev.currentTarget) return
    if (!target.value) {
        valueWasEmpty.value = true
        return
    }
    let newValue = '___-___-__-__'
    for (const char of target.value) {
        if (char.match(/\d/)) {
            newValue = newValue.replace('_', char)
        }
    }
    // get position of last number in newValue
    const lastNumberPos = newValue.indexOf('_')
    let offset = 0
    if (newValue[lastNumberPos - 1] === '-' && ev['data']) {
        offset++
    }
    // change input value but preserve cursor position
    const cursorPosition = target.selectionStart ?? 0
    setTimeout(() => {
        console.log(newValue)
        target.value = newValue
        const newPos = cursorPosition + offset
        target.setSelectionRange(newPos, newPos)
        valueWasEmpty.value = false
    }, valueWasEmpty.value ? 50 : 0)
}
</script>
<template>
    <h2>Personal Info</h2>
    <Form class="personal-info-form" :validationSchema="validationSchema" @submit="submit">
        <button hidden>submit</button>
        <Input autocapitalize="words" label="Full Name" name="fullName" required />
        <Input type="date" label="Birth Date" name="birthDate" />
        <Input type="email" label="Email" name="email" required />
        <Input label="City" name="city" />
        <Input type="tel" label="Phone" name="phone" @keydown="onlyNumbersKeydownHandler" @input="phoneNumberInputChange">
            <select name="phone-numbers" class="personal-info-form-select-phone-prefix">
                <!-- phone prefixes --->
                <option value="+1">+1 USA</option>
                <option value="+7">+7 Russia</option>
                <option value="+44">+44 UK</option>
                <option value="+33">+33 France</option>
                <option value="+49">+49 Germany</option>
                <option value="+34">+34 Spain</option>
                <option value="+39">+39 Italy</option>
                <option value="+46">+46 Sweden</option>
                <option value="+41">+41 Switzerland</option>
                <option value="+43">+43 Austria</option>
                <option value="+32">+32 Belgium</option>
                <option value="+31">+31 Netherlands</option>
                <option value="+48">+48 Poland</option>
                <option value="+45">+45 Denmark</option>
                <option value="+370">+370 Lithuania</option>
                <option value="+371">+371 Latvia</option>
                <option value="+372">+372 Estonia</option>
                <option value="+420">+420 Czech Republic</option>
                <option value="+421">+421 Slovakia</option>
                <option value="+386">+386 Slovenia</option>
                <option value="+385">+385 Croatia</option>
                <option value="+389">+389 Macedonia</option>
                <option value="+357">+357 Cyprus</option>
                <option value="+30">+30 Greece</option>
                <option value="+351">+351 Portugal</option>
                <option value="+1">+1 Canada</option>
                <option value="+55">+55 Brazil</option>
                <option value="+52">+52 Mexico</option>
                <option value="+64">+64 New Zealand</option>
                <option value="+61">+61 Australia</option>
                <option value="+82">+82 South Korea</option>
                <option value="+81">+81 Japan</option>
                <option value="+86">+86 China</option>
                <option value="+91">+91 India</option>
                <option value="+62">+62 Indonesia</option>
                <option value="+63">+63 Philippines</option>
                <option value="+84">+84 Vietnam</option>
                <option value="+7">+7 Kazakhstan</option>
            </select>
        </Input>
        <Input name="languages" label="Spoken languages">
            <template v-slot:input>
                <select class="personal-info-form--select-langs" @change="changeSelect">
                    <!-- a few languages with short codes -->
                    <option value="" disabled selected>Not selected</option>
                    <option v-for="[code, name] of spokenLanguages" :value="code" :disabled="selectedSpokenLanguages.includes(name)">{{ name }}</option>
                </select>
            </template>
        </Input>
        <div class="personal-info-form-spoken-languages--container">
            <div v-for="lang in selectedSpokenLanguages">
                <svg viewBox="0 0 24 24" class="personal-info-form-spoken-languages--delete" @click="removeSelectedItem(lang)">
                    <path d="M19 6.41 17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"></path>
                </svg>
                <span>{{ lang }}</span>
            </div>
        </div>
        <SubmitButton>Update info</SubmitButton>
    </Form>
</template>
<style scoped lang="scss">
.personal-info-form-select-phone-prefix {
    width: 80px;
}
.personal-info-form--select-langs {
    width: 100%;
    height: 2rem;
    border-radius: 5px;
}
.personal-info-form-spoken-languages--container {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
    margin-top: 5px;
    & > div {
        padding: 5px;
        border-radius: 5px;
        background-color: rgba(30, 143, 255, 0.3);
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
.personal-info-form-spoken-languages--delete {
    fill: rgb(212, 0, 0);
    width: 20px;
    cursor: pointer;
}
</style>
