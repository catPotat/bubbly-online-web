<template>
    <div class="the_big_frame" id="mbrshp-ls">
        <div class="common_ls_cntainr --top-lev-app-bar" ref="feed">
            <CmtyItem v-for="community in listDisplay"
                :key="community.id"
                :community="community"
            />
        </div>
        <FAB @clicked="$router.push('/communities/create')"
            icon= "add_circle_outline"
            actionName= "Add a community"
        />
    </div>
</template>

<script>
import { maintainScrllPos, scrlDirection } from '@/mixins/feedingFrenzy'
import FAB from '@/components/misc/FAB'
import CmtyItem from './list/CmtyItem'

export default {
    components: {CmtyItem, FAB},
    mixins: [maintainScrllPos, scrlDirection],
    computed: {
        listDisplay() {
            return this.$store.state.communityx.joined
        },
    },
    created() {
        this.$store.dispatch("communityx/getJoinedCmties")
    },
}
</script>
