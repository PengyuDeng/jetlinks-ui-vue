<template>
    <j-input
        v-if="type === 'string'"
        v-model:value="myValue"
        :maxLength="64"
        placeholder="请输入"
        @change="change"
    />
    <j-input-number
        v-else-if="type === 'int'"
        v-model:value="myValue"
        :precision="0"
        :max="2147483647"
        :min="-2147483647"
        style="width: 100%"
        placeholder="请输入"
        @change="change"
    />
    <j-input-number
        v-else-if="type === 'long'"
        v-model:value="myValue"
        :max="9223372036854775807"
        :min="-9223372036854775808"
        :precision="0"
        placeholder="请输入"
        style="width: 100%"
        @change="change"
    />
    <j-input-number
        v-else-if="['float', 'double'].includes(type)"
        v-model:value="myValue"
        :max="9999999999999999"
        :min="-9999999999999999"
        placeholder="请输入"
        style="width: 100%"
        @change="change"
    />
    <j-select
        v-else-if="type === 'boolean'"
        placeholder="请选择"
        :options="[
            { label: '否', value: 'false'},
            { label: '是', value: 'true'},
        ]"
        :get-popup-container="(node) => fullRef || node"
    />
    <j-date-picker
        v-else-if="type === 'date' "
        v-model:value="myValue"
        show-time
        placeholder="请选择"
        :get-popup-container="(node) => fullRef || node"
        @change="change"
    />
</template>
<script setup name="MetricValueItem">
import { Form } from 'jetlinks-ui-components'
import {FULL_CODE} from "jetlinks-ui-components/es/DataTable";

const props = defineProps({
    value: {
        type: [String, Number, Array],
        default: undefined
    }
})

const emit = defineEmits(['update:value'])
const formItemContext = Form.useInjectFormItemContext();

const type = inject('metricsType')
const myValue = ref(props.value)
const fullRef = inject(FULL_CODE);

const change = () => {
    // formItemContext.onFieldChange()
    emit('update:value', myValue.value)
}

watch(() => props.value, () => {
    myValue.value = props.value
})

</script>