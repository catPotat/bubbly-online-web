<template>
    <div :class="['thrd glow', isActive?'room--active': null]"
        @click="$store.dispatch('chatx/toChat', room)">
        <img :class="isPublic?'cmty_ico':'pfp'" :src="roomPic">
        <div v-if="isUnread" class="thrd__dot"></div>
        <div class="thread-txt">
            <div class="thrd__top-txt">
                <p class="thrd__top__title" :style="[room.room_type=='direct' ? {'color':'#'+room.room_type_data.fave_color} :null]"
                >{{ roomTitle }}</p>
                <span>{{ room.last_msg.timestamp | tiemstamp2 }}</span>
            </div>
            <p class="thrd__last-msg" :style="isUnread? 'font-weight:bold; color:black' :null">
                {{ renderLastMsg(room.last_msg) }}
            </p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['room'],
    computed: {
        isDirect() { return this.room.room_type == 'direct' },
        isGroup() { return this.room.room_type == 'group' },
        isPublic() { return this.room.room_type == 'public' },
        roomPic() {
            const dat = this.room.room_type_data
            return this.isDirect ? dat.profile_pic :
                this.room.bg_img ? this.room.bg_img :
                this.isPublic ? dat.community.icon_img :
                require('@/assets/group.png')
        },
        roomTitle() {
            const dat = this.room.room_type_data
            return this.isDirect ? dat.alias :
                this.room.name ? this.room.name :
                this.isGroup ? "Nhóm " + dat.roommate_count + " người" :
                this.isPublic ? dat.community.name + ` #${dat.order}` : ""
        },
        isUnread() {
            if (this.isActive) return false
            return new Date(this.room.roommate_info.last_seen) < new Date(this.room.last_msg.timestamp)
        },
        isActive() {
            return this.$route.query.room == this.room.id
        }
    }
}
</script>


<style>
.room--active {
    background: #d3d3d3 !important;
    cursor: context-menu;
}
.thrd {
    padding: 7.5px 15px;
    word-wrap: break-word;
    display: flex;
    align-items: center;
    width: 100%;
    border-radius: 15px;
    position: relative;
}
.thrd .pfp, .thrd .cmty_ico {
    align-self: flex-start;
    /* z-index: -2; */
    height: 55px;
    min-width: 55px;
    max-width: 55px;
}
.thread-txt {
    margin-left: 15px;
    width: 100%;
}
.thread-txt .thrd__top-txt{
    display: flex;
    width: 100%;
}
.thrd__top-txt .thrd__top__title {
    font-weight: bold;
    word-break: break-all;
    
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical; 
}
.thrd__top-txt > span {
    color: #888;
    margin: 2px 0 0 auto;
    font-size: 14px;
    padding-left: 10px;
}
.thread-txt .thrd__last-msg{
    color: #777;
    font-size: 14px;
    
    word-break: break-all;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;

    max-height: 70px;
}

.thrd__dot {
    position: absolute;
    top: 6px;
    left: 57px;
    height: 15px;
    width: 15px;
    border-radius: 50%;
    background: var(--primary-color);
    opacity: 0.95;
}
</style>
