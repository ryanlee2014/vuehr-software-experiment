<template>
    <div>
        <div style="display: flex;justify-content: space-between">
            <div>
                <el-button type="primary" icon="el-icon-plus" @click="addMapVisible = true">
                    Add Map
                </el-button>
            </div>
        </div>
        <div>
            <el-table
                    :data="mapInfo"
                    style="width: 100%">
                <el-table-column
                        prop="name"
                        label="Name"
                        width="180">
                </el-table-column>
                <el-table-column
                        label="Create Time"
                        width="180">
                    <template slot-scope="scope">
                        {{dayjs(scope.row.createTime).format('YYYY-MM-DD HH:mm:ss')}}
                    </template>
                </el-table-column>
                <el-table-column
                        prop="tag"
                        label="Tag">
                </el-table-column>
                <el-table-column
                        fixed="right"
                        label="Operation"
                        width="100"
                >
                    <template slot-scope="scope">
                        <el-button @click="showMap(scope.row)" type="text" size="small"
                        >
                            Show map
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

        </div>
        <el-dialog
                title="Add Map"
                :visible.sync="addMapVisible"
                width="30%">
            <el-form class="">
                <el-form-item label="name">
                    <el-input v-model="dialogAddMap.name"></el-input>
                </el-form-item>
                <el-form-item label="tag">
                    <el-input v-model="dialogAddMap.tag"></el-input>
                </el-form-item>
                <el-form-item label="latitude">
                    <el-input v-model="dialogAddMap.latitude"></el-input>
                </el-form-item>
                <el-form-item label="longitude">
                    <el-input v-model="dialogAddMap.longitude"></el-input>
                </el-form-item>
                <el-form-item label="scale">
                    <el-input v-model="dialogAddMap.scale"></el-input>
                </el-form-item>
                <l-map style="height: 350px;" :zoom="showMapInfo.scale"
                       :center="[dialogAddMap.latitude, dialogAddMap.longitude]"
                @update:zoom="onZoomUpdated"
                @update:center="onCenterUpdated">
                    <l-tile-layer :url="url"></l-tile-layer>
                    <l-marker :lat-lng="[dialogAddMap.latitude, dialogAddMap.longitude]"></l-marker>
                </l-map>
                <el-form-item>
                    <el-button type="primary" @click.prevent="addMap" style="margin-top: 20px">Submit</el-button>
                </el-form-item>
            </el-form>

        </el-dialog>
        <el-dialog
                title="Map Browse"
                :visible.sync="mapBrowseVisible"
                width="70%">
            <l-map style="height: 350px;" :zoom="showMapInfo.scale"
                   :center="[showMapInfo.latitude, showMapInfo.longitude]">
                <l-tile-layer :url="url"></l-tile-layer>
                <l-marker :lat-lng="[showMapInfo.latitude, showMapInfo.longitude]"></l-marker>
            </l-map>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    import Vue from "vue";
    import {Component} from "vue-property-decorator";
    import {getRequest, postRequest} from "@/utils/api";
    import dayjs from "dayjs";
    import {LMap, LTileLayer, LMarker} from 'vue2-leaflet';

    interface IMapInfoDTO {
        name: string,
        createTime: string,
        status: number,
        type: number,
        tag: string,
        latitude: number,
        longitude: number,
        scale: number
    }

    const emptyMapInfo: IMapInfoDTO = {
        createTime: "",
        latitude: 0,
        longitude: 0,
        name: "",
        scale: 0,
        status: 1,
        tag: "",
        type: 1
    };

    @Component({
        components: {
            LMap,
            LTileLayer,
            LMarker
        }
    })
    export default class MapConfig extends Vue {
        dayjs = dayjs;
        addMapVisible = false;
        mapBrowseVisible = false;
        mapInfo: IMapInfoDTO[] = [];
        showMapInfo: IMapInfoDTO = Object.assign({}, emptyMapInfo);
        url = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
        dialogAddMap: IMapInfoDTO = {
            createTime: "",
            latitude: 0,
            longitude: 0,
            name: "",
            scale: 0,
            status: 1,
            tag: "",
            type: 1
        };

        initData() {
            getRequest("/map/all")
                .then(resp => {
                    this.mapInfo = resp as any;
                })
        }

        mounted() {
            this.initData();
        }

        showMap(row: any) {
            this.showMapInfo = row;
            this.mapBrowseVisible = true;
        }

        addMap() {
            postRequest("/map/insert", this.dialogAddMap)
                .then(resp => {
                    this.initData();
                    Object.assign(this.dialogAddMap, emptyMapInfo);
                })
        }

        onZoomUpdated (zoom: any) {
            this.dialogAddMap.scale = zoom;
        }

        onCenterUpdated (center: any) {
            this.dialogAddMap.latitude = center.lat;
            this.dialogAddMap.longitude = center.lng;
        }

        deleteMap() {

        }
    }
</script>

<style scoped>

</style>
