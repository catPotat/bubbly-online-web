<template>
    <div class="the_big_frame">
        <div class="react-ls common_ls_cntainr --with-tabs" ref="feed">
            <Tabs locked
                :tabs="['ALL', ...nameLs]"
                :currentTab="currentTab"
                @switchTo="newTab"
            />
            <transition-group name="zoom_in_fade" style="width:100%">
                <UserItem v-for="profile in fetchedData"
                    :key="profile.username"
                    :profile="profile"
                />
            </transition-group>
            <StatusIndicator :isFetching="loading4More" :listLen="fetchedData.length"/>
        </div>
    </div>
</template>

<script>
import { tabs } from '@/mixins/cmpnentsCtrl/tabs'
import { feedingFrenzy } from '@/mixins/feedingFrenzy'
import UserItem from '@/components/profile/list/UserItem'

import { mapGetters } from "vuex"
export default {
    components: { UserItem },
    mixins: [tabs, feedingFrenzy],
    data:() => ({
        reactsAggregate: [],
    }),
    computed: {
        ...mapGetters({
            emoteById: 'reactionx/emoteById',
        }),
        currentEmote() {
            const index = this.currentTab-1
            if (index != -1) {
                return this.reactsAggregate[index].icon_id
            } return ''
        },
        feedUrl() {
            return `posts/${this.$route.params.postId}/reacts/?emote=${this.currentEmote}&`
        },
        nameLs() {
            const obj = this.reactsAggregate            
            return Object.keys(obj).map(key => `${
                obj[key]
                // this.emoteById(this.$store.state.postx.currentPost.allocated_to.id, obj[key].icon_id)
            .name} (${obj[key].count})`)
        }
    },
    watch: {
        currentTab() {
            this.firstFetch()
        },
    },
    created() {
        this.$axios.get(
            `posts/${this.$route.query.comment==1?'comment/':''}${this.$route.params.postId}`,
            this.$store.state.auth.head
        )
            .then(res => {
                this.reactsAggregate = res.data.reactions
            })
    }
}
</script>
