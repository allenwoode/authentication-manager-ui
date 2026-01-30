<template>
  <div class="product-container">
    <pro-search
      :columns="columns"
      target="category-device"
      noMargin
      @search="(params:any)=>queryParams = {...params}"
      ref="searchRef"
    />
    <FullPage>
      <j-pro-table
        ref="tableRef"
        :request="table.requestFun"
        modeValue="CARD"
        :gridColumn="2"
        :gridColumns="[2]"
        :params="queryParams"
        :rowSelection="{
            selectedRowKeys: table._selectedRowKeys.value,
            onSelect: table.onSelect,
            onSelectAll: table.onSelectAll,
            onSelectNone: table.cancelSelect,
        }"
        :columns="columns"
      >
        <template #headerLeftRender>
          <a-space>
            <j-permission-button
              :hasPermission="`${permission}:assert`"
              type="primary"
              @click="table.clickAdd('handle')"
            >
              <AIcon type="PlusOutlined"/>
              {{ $t('device.index.988419-0') }}
            </j-permission-button>

            <j-permission-button
                      :hasPermission="`${permission}:bind`"
                      :popConfirm="{
                        title: $t('device.index.988419-2'),
                        onConfirm: () =>
                          table.clickUnBind(),
                      }"
                    >
                      <AIcon
                        type="DisconnectOutlined"
                      />
                      {{ $t('device.index.988419-3') }}
                    </j-permission-button>
            <!-- <a-dropdown trigger="hover">
              <a-button>{{ $t('device.index.988419-1') }}</a-button>
              <template #overlay>
                <a-menu>
                  <a-menu-item>
                    <j-permission-button
                      :hasPermission="`${permission}:bind`"
                      :popConfirm="{
                        title: $t('device.index.988419-2'),
                        onConfirm: () =>
                          table.clickUnBind(),
                      }"
                    >
                      <AIcon
                        type="DisconnectOutlined"
                      />
                      {{ $t('device.index.988419-3') }}
                    </j-permission-button>
                  </a-menu-item>
                  <a-menu-item>
                    <j-permission-button
                      :hasPermission="`${permission}:assert`"
                      @click="table.clickEdit()"
                    >
                      <AIcon
                        type="EditOutlined"
                      />
                      {{ $t('device.index.988419-4') }}
                    </j-permission-button>
                  </a-menu-item>
                </a-menu>
              </template>
            </a-dropdown> -->
          </a-space>
        </template>

        <template #card="slotProps">
          <CardBox
                        :value="slotProps"
                        :actions="table.getActions(slotProps, 'card')"
                        v-bind="slotProps"
                        :active="
                            table._selectedRowKeys.value.includes(slotProps.id)
                        "
                        @click="table.onSelectChange"
                        :status="slotProps.state?.value"
                        :statusText="slotProps.state?.text"
                        :statusNames="{
                            online: 'processing',
                            offline: 'error',
                        }"
                    >
                        <template #img>
                            <slot name="img">
                                <img
                                    :src="slotProps.photoUrl"
                                    style="cursor: pointer; width: 80px; height: 80px"
                                />
                            </slot>
                        </template>
                        <template #content>
                            <h3 class="card-item-content-title" style='margin-bottom: 18px;'>
                                {{ slotProps.name }}
                            </h3>
                            <a-row>
                                <a-col :span="12">
                                    <div class="card-item-content-text">ID</div>
                                    <j-ellipsis style="width: calc(100% - 20px);">
                                    <div
                                        style="cursor: pointer"
                                        class="card-item-content-value"
                                    >
                                        {{ slotProps.id }}
                                    </div>
                                    </j-ellipsis>
                                </a-col>
                                <a-col :span="12">
                                    <div class="card-item-content-text">
                                        {{ $t('product.index.083446-19') }}
                                    </div>
                                    <j-ellipsis style="width: calc(100% - 20px);">
                                    <div class="card-item-content-value">
                                        {{ slotProps.productName }}
                                    </div>
                                    </j-ellipsis>
                                </a-col>
                            </a-row>
                        </template>
                        <template #actions="item">
                            <a-tooltip
                                v-bind="item.tooltip"
                                :title="item.disabled && item.tooltip.title"
                            >
                                <a-dropdown
                                    placement="bottomRight"
                                    v-if="item.key === 'others'"
                                >
                                    <a-button>
                                        <AIcon :type="item.icon" />
                                        <span>{{ item.text }}</span>
                                    </a-button>
                                    <template #overlay>
                                        <a-menu>
                                            <a-menu-item
                                                v-for="(o, i) in item.children"
                                                :key="i"
                                            >
                                                <a-button
                                                    type="link"
                                                    @click="o.onClick"
                                                >
                                                    <AIcon :type="o.icon" />
                                                    <span>{{ o.text }}</span>
                                                </a-button>
                                            </a-menu-item>
                                        </a-menu>
                                    </template>
                                </a-dropdown>
                                <j-permission-button
                                    v-else
                                    :hasPermission="item.permission"
                                    :tooltip="item.tooltip"
                                    :pop-confirm="item.popConfirm"
                                    @click="item.onClick"
                                    :disabled="item.disabled"
                                >
                                    <AIcon :type="item.icon" />
                                    <span v-if="item.key !== 'delete'">{{
                                        item.text
                                    }}</span>
                                </j-permission-button>
                            </a-tooltip>
                        </template>
                    </CardBox>
          <!-- <CardBox
            :value="slotProps"
            :actions="[{ key: 1 }]"
            v-bind="slotProps"
            :active="
              table._selectedRowKeys.value.includes(slotProps.id)
            "
            @click="table.onSelectChange"
            :status="slotProps.state?.value"
            :statusText="slotProps.state?.text"
            :statusNames="{
              online: 'processing',
              offline: 'error',
              notActive: 'warning',
            }"
          >
            <template #img>
              <slot name="img">
                <img
                  :src="systemImg.deviceProductImg"
                  style="cursor: pointer"
                  alt=""
                />
              </slot>
            </template>
            <template #content>
              <h3 class="card-item-content-title" style='margin-bottom: 18px;'>
                {{ slotProps.name }}
              </h3>
              <a-row>
                <a-col :span="12">
                  <div class="card-item-content-text">ID</div>
                  <j-ellipsis style="width: calc(100% - 20px);">
                    <div
                      style="cursor: pointer"
                      class="card-item-content-value"
                    >
                      {{ slotProps.id }}
                    </div>
                  </j-ellipsis>
                </a-col>
                <a-col :span="12">
                  <div class="card-item-content-text">
                    {{ $t('device.index.988419-5') }}
                  </div>
                  <j-ellipsis style="width: calc(100% - 20px);">
                    <div
                      style="cursor: pointer"
                      class="card-item-content-value"
                    >
                      {{
                        table.permissionList.value.length &&
                        table.getPermissLabel(
                          slotProps.permission,
                        )
                      }}
                    </div>
                  </j-ellipsis>
                </a-col>
              </a-row>
            </template>
            <template #actions>
              <j-permission-button
                :hasPermission="`${permission}:assert`"
                @click="table.clickEdit(slotProps)"
              >
                <AIcon type="EditOutlined"/>
              </j-permission-button>

              <j-permission-button
                :hasPermission="`${permission}:bind`"
                :popConfirm="{
                  title: $t('device.index.988419-6'),
                  onConfirm: () =>
                    table.clickUnBind(slotProps),
                }"
              >
                <AIcon type="DisconnectOutlined"/>
              </j-permission-button>
            </template>
          </CardBox> -->
        </template>

        <template #permission="slotProps">
          {{
            table.permissionList.value.length &&
            table.getPermissLabel(slotProps.permission)
          }}
        </template>
        <template #state="slotProps">
          <JBadgeStatus
            :status="slotProps.state.value"
            :text="slotProps.state.text"
            :statusNames="{
              online: 'processing',
              offline: 'error',
              notActive: 'warning',
            }"
          ></JBadgeStatus>
        </template>
        <template #createTime="slotProps">
        <span>{{
            slotProps.createTime ? dayjs(slotProps.createTime).format(
              'YYYY-MM-DD HH:mm:ss',
            ) : '-'
          }}</span>
        </template>
        <template #action="slotProps">
          <a-space :size="16">
            <j-permission-button
              v-for="i in table.getActions(slotProps, 'table')"
              :hasPermission="i.permission"
              type="link"
              :tooltip="i?.tooltip"
              :pop-confirm="i.popConfirm"
              @click="i.onClick"
              :disabled="i?.disabled"
            >
              <AIcon :type="i.icon"/>
            </j-permission-button>
          </a-space>
        </template>
      </j-pro-table>
    </FullPage>

    <div class="dialogs">
      <DeviceDialog
        v-if="dialogs.addShow"
        v-model:visible="dialogs.addShow"
        :query-columns="columns"
        :parent-ids="parentIds"
        :parent-id="parentId"
        :pparent-id="pparentId"
        :all-permission="table.permissionList.value"
        asset-type="device"
        @confirm="table.refresh"
      />
      <EditPermissionDialog
        v-if="dialogs.editShow"
        v-model:visible="dialogs.editShow"
        :ids="dialogs.selectIds"
        :permission-list="dialogs.permissList"
        :parent-id="props.parentId"
        :all-permission="table.permissionList.value"
        asset-type="device"
        :defaultPermission="table.defaultPermission"
        @confirm="table.refresh"
      />
    </div>
  </div>
</template>

<script setup lang="ts" name="device">
import DeviceDialog from '../components/DeviceDialog.vue';
import EditPermissionDialog from '../components/EditPermissionDialog.vue';
import {onlyMessage} from '@/utils/comm';
import {
  getDeviceAssetList_api,
  //getPermission_api,
  getPermissionDict_api,
  unBindDeviceOrProduct_api,
  getDeviceProduct_api,
  getBindingsPermission,
} from '@authentication-manager/api/system/department';
import {intersection} from 'lodash-es';

import type {dictType} from '../typings';
import {useDepartmentStore} from '@/store/department';
import dayjs from 'dayjs';
//import {systemImg} from "@authentication-manager/assets";
import { useI18n } from 'vue-i18n';

const { t: $t } = useI18n();
const departmentStore = useDepartmentStore();

const permission = 'system/Department';

const emits = defineEmits(['update:bindBool']);
const props = defineProps<{
  parentIds: string[];
  parentId: string;
  pparentId: string;
  bindBool: boolean;
}>();
const columns = [
  {
    title: 'ID',
    dataIndex: 'id',
    key: 'id',
    ellipsis: true,
    fixed: 'left',
    search: {
      type: 'string',
    },
    width: 200
  },
  {
    title: $t('device.index.988419-7'),
    dataIndex: 'name',
    key: 'name',
    ellipsis: true,
    search: {
      type: 'string',
      first: true,
    },
  },
  {
    title: $t('device.index.988419-8'),
    dataIndex: 'productName',
    key: 'productName',
    ellipsis: true,
    search: {
      rename: 'productId$product-info',
      type: 'select',
      handleValue(value: string, data: any) {
        return value && value.length ? [{
          column: 'id',
          termType: data?.termType === 'not' ? 'nin' : 'in',
          value: `${value.toString()}`
        }] : undefined;
      },
      options: () =>
        new Promise((resolve) => {
          const params = {
            paging: false,
            'sorts[0].name': 'createTime',
            'sorts[0].order': 'desc',
          };
          getDeviceProduct_api(params).then((resp: any) => {
            const result = resp.result.map((item: any) => ({
              label: item.name,
              value: item.id,
            }));
            resolve(result);
          });
        }),
    },
  },
  // {
  //   title: $t('device.index.988419-5'),
  //   dataIndex: 'permission',
  //   key: 'permission',
  //   ellipsis: true,
  //   width: 300,
  //   scopedSlots: true,
  // },
  {
    title: $t('device.index.988419-9'),
    dataIndex: 'createTime',
    key: 'createTime',
    ellipsis: true,
    scopedSlots: true,
    width: 200,
    search: {
      type: 'date',
    },
  },
  {
    title: $t('device.index.988419-10'),
    dataIndex: 'state',
    key: 'state',
    ellipsis: true,
    search: {
      type: 'select',
      options: [
        {label: $t('device.index.988419-11'), value: 'notActive'},
        {label: $t('device.index.988419-12'), value: 'offline'},
        {label: $t('device.index.988419-13'), value: 'online'},
      ],
    },
    scopedSlots: true,
    width: 120
  },
  {
    title: $t('device.index.988419-14'),
    dataIndex: 'action',
    key: 'action',
    fixed: 'right',
    width: 100,
    scopedSlots: true,
  },
];
const queryParams = ref({});

const tableRef = ref();
const searchRef = ref();
const table = {
  _selectedRowKeys: ref<string[]>([]),
  selectedRows: [] as any[],
  permissionList: ref<dictType>([]),
  defaultPermission: [] as string[],

  init: () => {
    //table.getPermissionDict();
    watch(
      () => props.parentId,
      () => {
        table.refresh();
        searchRef.value?.reset?.()
      },
    );
  },

  getActions: (
    data: Partial<Record<string, any>>,
    type: 'card' | 'table',
  ) => {
    if (!data) return [];
    else
      return [
        // {
        //   permission: `${permission}:assert`,
        //   key: 'edit',
        //   tooltip: {title: $t('device.index.988419-15')},
        //   icon: 'EditOutlined',
        //   onClick: () => table.clickEdit(data),
        // },
        {
          permission: `${permission}:bind`,
          key: 'unbind',
          tooltip: {title: $t('device.index.988419-16')},
          popConfirm: {
            title: $t('device.index.988419-6'),
            onConfirm: () => table.clickUnBind(data),
          },
          icon: 'DisconnectOutlined',
        },
      ];
  },
  // 获取权限数据字典
  getPermissionDict: () => {
    getPermissionDict_api().then((resp: any) => {
      table.permissionList.value = resp.result;
    });
  },
  // 获取权限名称
  getPermissLabel: (values: string[]) => {
    const permissionList = table.permissionList.value;
    if (permissionList.length < 1 || values.length < 1) return '';
    const result = values.map(
      (key) => permissionList.find((item) => item.id === key)?.name,
    );
    return result.join(',');
  },
  // 选中
  onSelectChange: (row: any) => {
    const selectedRowKeys = table._selectedRowKeys.value;
    const index = selectedRowKeys.indexOf(row.id);

    if (index === -1) {
      selectedRowKeys.push(row.id);
      table.selectedRows.push(row);
    } else {
      selectedRowKeys.splice(index, 1);
      table.selectedRows.splice(index, 1);
    }
  },
  // 取消全选
  cancelSelect: () => {
    table._selectedRowKeys.value = [];
    table.selectedRows = [];
  },

  onSelect: (record: any, selected: boolean) => {
    const arr = [...table._selectedRowKeys.value]
    const _index = arr.findIndex(item => item === record?.id)
    if (selected) {
      if (!(_index > -1)) {
        table._selectedRowKeys.value.push(record.id)
        table.selectedRows.push(record)
      }
    } else {
      if (_index > -1) { // 去掉数据
        table._selectedRowKeys.value.splice(_index, 1)
        table.selectedRows.splice(_index, 1)
      }
    }
  },

  onSelectAll: (selected: boolean, _: any[], changeRows: any) => {
    if (selected) {
      changeRows.map((i: any) => {
        if (!table._selectedRowKeys.value.includes(i.id)) {
          table._selectedRowKeys.value.push(i.id)
          table.selectedRows.push(i)
        }
      })
    } else {
      const arr = changeRows.map((item: any) => item.id)
      const _arr: string[] = [];
      const _ids: string[] = [];
      [...table.selectedRows].map((i: any) => {
        if (!arr.includes(i?.id)) {
          _arr.push(i)
          _ids.push(i.id)
        }
      })
      table._selectedRowKeys.value = _ids
      table.selectedRows = _arr
    }
  },

  // 获取并整理数据
  getData: (params: object, parentId: string) =>

    new Promise((resolve) => {
      getDeviceAssetList_api(params).then((resp) => {
        type resultType = {
          data: any[];
          total: number;
          pageSize: number;
          pageIndex: number;
        };
        const {pageIndex, pageSize, total, data} = resp.result as resultType;
        const ids = data.map((item) => item.id);
        data.forEach((item) => (item.permission = {}));

        resolve({
              code: 200,
              result: {
                data: data,
                pageIndex,
                pageSize,
                total,
              },
              status: 200,
            });

        // getPermission_api('device', ids, parentId).then(
        //   (perResp: any) => {
        //     const permissionObj = {};
        //     perResp.result.forEach((item: any) => {
        //       permissionObj[item.assetId] =
        //         item.grantedPermissions;
        //     });
        //     data.forEach(
        //       (item) =>
        //         (item.permission = permissionObj[item.id]),
        //     );

        //     resolve({
        //       code: 200,
        //       result: {
        //         data: data,
        //         pageIndex,
        //         pageSize,
        //         total,
        //       },
        //       status: 200,
        //     });
        //   },
        // );
      });
    }),
  // 整理参数并获取数据
  requestFun: async (oParams: any) => {
    if (props.parentId) {
      const params = {
        ...oParams,
        sorts: [{name: 'id', order: 'desc'}],
        terms: [
          ...oParams.terms,
            {
              //"column": "id$in-dim-asset$org$device",
              "column": "dimensionId",
              "termType": "eq",
              "value": props.parentId
            },
        ],
      };
      const resp: any = await table.getData(params, props.parentId);
      return {
        code: resp.status,
        result: resp.result,
        status: resp.status,
        success: true
      };
    } else {
      return {
        code: 200,
        result: {
          data: [],
          pageIndex: 0,
          pageSize: 0,
          total: 0,
        },
        status: 200,
      };
    }
  },

  clickAdd: (type?: string) => {
    // 设备资产分配弹窗操作类型: type = 'handle': 手动点击资产分配按钮, !type产品资产分配后, 自动弹出设备资产分配
    departmentStore.setType(type);
    dialogs.addShow = true;
  },

  queryPermissionList: async (ids: string[]) => {
    const resp: any = await getBindingsPermission('device', ids)
    if (resp.status === 200) {
      const arr = resp.result.map((item: any) => {
        return item?.permissionInfoList?.map((i: any) => i?.id)
      })
      return intersection(...arr)
    }
    return []
  },

  clickEdit: async (row?: any) => {
    const ids = row ? [row.id] : [...table._selectedRowKeys.value];
    if (ids.length < 1) return onlyMessage($t('device.index.988419-17'), 'warning');

    table.defaultPermission = row ? row?.permission : intersection(...table.selectedRows.map(
      (item) => item.permission,
    )) as string[]

    const _result = await table.queryPermissionList(ids)
    dialogs.selectIds = ids;
    dialogs.permissList = _result as string[];
    dialogs.editShow = true;
  },

  clickUnBind: (row?: any) => {
    const ids = row ? [row.id] : [...table._selectedRowKeys.value];
    if (ids.length < 1) return onlyMessage($t('device.index.988419-18'), 'warning');

    const response = unBindDeviceOrProduct_api(props.parentId, 'device', ids)
    response.then(() => {
      onlyMessage($t('device.index.988419-19'));
      table.refresh();
    });
    return response
  },

  refresh: () => {
    nextTick(() => {
      tableRef.value.reload();
      table.cancelSelect()
    });
  },
};

const dialogs = reactive({
  selectIds: [] as string[],
  permissList: [] as string[],
  addShow: false,
  editShow: false,
});

table.init();
watchEffect(() => {
  props.bindBool && table.clickAdd();
  emits('update:bindBool', false);
});
</script>

<style lang="less" scoped>
.product-container {
  .card {
    .card-warp {
      &.active {
        .card-item-content-value {
          color: #2f54eb;
        }
      }
    }

    .card-tools {
      span {
        color: #252525;
      }
    }
  }
}
</style>
