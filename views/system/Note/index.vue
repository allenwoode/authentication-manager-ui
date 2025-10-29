<template>
    <j-page-container>
        <h1>Note Page Index</h1>
        <div>
        {{ route.name }}
        </div>
        <j-permission-button
        type="primary"
        hasPermission="system/Note:add"
        @click="() => { status = status == 1 ? 0 : 1 }"
        >
            <template #icon>
                123456
            </template>
            {{ status }}
        </j-permission-button>
    </j-page-container>
</template>

<script lang="ts" setup>
import { useRoute } from 'vue-router'
const route = useRoute()

const status = ref(1)
</script>