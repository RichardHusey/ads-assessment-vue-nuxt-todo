<template>
    <v-list-item value="data.value">
        <template v-slot:default="{ active, toggle }">
            <v-list-item-action>
                <v-checkbox :input-value="active" color="primary" @click="toggle" />
            </v-list-item-action>
            <v-list-item-content>
                <v-list-item-title class="d-flex justify-space-between"><span v-html="data.subtitle"></span> <small>{{
                    data.score
                }}
                        pts.</small></v-list-item-title>
                <v-list-item-subtitle>
                    <p v-html="data.subtitle"></p>
                </v-list-item-subtitle>
            </v-list-item-content>
        </template>
    </v-list-item>
</template>

<script>

export default {
    props: {
        data: {
            value: String,
            title: String,
            score: Number,
            subtitle: String
        }
    },
    data() {
        return {
            fixed: false,
        }
    },
}
</script>
