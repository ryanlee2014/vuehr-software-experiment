<template>
    <div>
        <div style="display: flex;justify-content: space-between">
            <div>
                <el-button type="primary" icon="el-icon-plus" @click="addNoteVisible = true">
                    Add Note
                </el-button>
            </div>
        </div>
        <el-card class="box-card" style="margin-top: 20px" v-for="row in noteList" :key="row.noteId">
            <div slot="header" class="clearfix">
                <span>{{row.noteId}}</span>
            </div>
            <div  class="text item">
                {{row.content}}
            </div>
        </el-card>
        <el-dialog
                title="Add Note"
                :visible.sync="addNoteVisible"
                width="30%">
            <el-form class="">
                <el-form-item label="name">
                    <el-input v-model="content" type="textarea"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click.prevent="addNote">Submit</el-button>
                </el-form-item>
            </el-form>

        </el-dialog>

    </div>
</template>

<script lang="ts">
    import Vue from "vue";
    import {Component} from "vue-property-decorator";
    import {getRequest, postRequest} from "@/utils/api";
    interface INote {
        noteId: number,
        content: string
    }
    @Component
    export default class Note extends Vue {
        addNoteVisible = false;
        noteList: INote[] = [];
        content = "";
        mounted () {
            this.initData();
        }

        initData () {
            getRequest("/note/all")
                .then(resp => {
                    this.noteList = resp as any;
                })
        }

        addNote () {
            postRequest("/note/insert", {content: this.content})
                .then(resp => {
                    this.initData();
                    this.addNoteVisible = false;
                })
        }
    }
</script>

<style scoped>

</style>
