<template>
    <section id="tables-list" data-el="tables-list">
        <div class="header flex gap-2">
            <ff-text-input
                v-model="filterTerm"
                class="ff-data-table--search"
                data-form="search"
                placeholder="Search Tables"
            >
                <template #icon><SearchIcon /></template>
            </ff-text-input>
            <button class="ff-btn ff-btn--secondary transition-fade--color" type="button" @click.stop="onCreateTable">
                <span class="ff-btn--icon">
                    <PlusIcon />
                </span>
            </button>
        </div>

        <ul v-if="filteredTables.length && tables.length" class="list">
            <li
                v-for="table in filteredTables" :key="table.id"
                :title="table.name"
                class="item relative"
                :class="{active: table.name === tableSelection}"
                @click="updateTableSelection(table.name)"
            >
                <span class="icon-toggle">
                    <TableIcon class="ff-icon ff-icon-sm" />
                    <PencilAltIcon class="ff-icon ff-icon-sm edit" @click="showSchema(table)" />
                </span>
                <span class="truncate">{{ table.name }}</span>
            </li>
        </ul>

        <div v-else-if="!filteredTables.length && tables.length" class="empty-state">
            <p>No tables found matching your criteria!</p>
        </div>

        <div v-else class="empty-state flex gap-5">
            <p>Get started by creating your first table using the <code>@flowfuse/nr-tables-nodes</code> node in a Node-RED Instance.</p>
            <p>or <span class="cta" @click="onCreateTable">create</span> your first table now.</p>
        </div>
    </section>
</template>

<script>
import { PencilAltIcon, PlusIcon, SearchIcon, TableIcon } from '@heroicons/vue/outline'
import { defineComponent, markRaw } from 'vue'
import { mapActions, mapGetters, mapState } from 'vuex'

import CreateTable from '../drawers/CreateTable.vue'
import TableSchema from '../drawers/TableSchema.vue'

export default defineComponent({
    name: 'TablesList',
    components: { SearchIcon, TableIcon, PlusIcon, PencilAltIcon },
    emits: ['select-table'],
    data () {
        return {
            filterTerm: '',
            tables: []
        }
    },
    computed: {
        ...mapGetters('product/tables', { getTables: 'tables' }),
        ...mapState('product/tables', { tablesState: 'tables', tableSelection: 'tableSelection' }),
        ...mapState('ux/drawers', ['rightDrawer']),
        filteredTables () {
            return this.tables.filter(t => (t.name ?? '').toLowerCase().includes(this.filterTerm.toLowerCase()))
        }
    },
    watch: {
        tablesState: {
            deep: true,
            handler (newVal) {
                this.tables = this.getTables(this.$route.params.id)
            }
        }
    },
    methods: {
        ...mapActions('product/tables', ['updateTableSelection']),
        ...mapActions('ux/drawers', ['openRightDrawer', 'closeRightDrawer']),

        onCreateTable () {
            this.openRightDrawer({
                component: markRaw(CreateTable),
                wider: true,
                overlay: true
            })
        },
        showSchema (table) {
            this.openRightDrawer({
                component: markRaw(TableSchema),
                props: { table },
                overlay: true
            })
        }
    }
})
</script>

<style scoped>
#tables-list {
    display: flex;
    flex-direction: column;
    max-width: 20%;
    min-width: 250px;
}

#tables-list .header {
    border-bottom: 1px solid var(--color-border);
    padding-bottom: 15px;
    margin-bottom: 15px;
}

#tables-list .header .ff-data-table--search {
    min-width: 10px;
}

#tables-list .list .item {
    display: flex;
    gap: 5px;
    line-height: 2;
    align-items: center;
    transition: ease-in-out .3s;
    cursor: pointer;
}

#tables-list .list .item:hover,
#tables-list .list .item.active {
    color: var(--color-indigo-50)0;
    background-color: var(--color-gray-100);
}

#tables-list .list .item:hover .icon-toggle .ff-icon:first-child {
    display: none;
}

#tables-list .list .item:hover .icon-toggle .ff-icon:last-child {
    display: inline-block;
}

#tables-list .list .item .icon-toggle {
    width: 24px;
}

#tables-list .list .item .icon-toggle .ff-icon:first-child {
    display: inline-block;
}

#tables-list .list .item .icon-toggle .ff-icon:last-child {
    display: none;
}

#tables-list .list .item .icon-toggle .edit:hover {
    transform: scale(1.4);
}

#tables-list .empty-state {
    flex: 1;
    display: flex;
    flex-direction: column;
    text-align: center;
    justify-content: center;
    color: var(--color-gray-400);
    line-height: 1.6;
}

#tables-list .empty-state .cta {
    cursor: pointer;
    color: var(--color-indigo-50)0;
}
</style>
