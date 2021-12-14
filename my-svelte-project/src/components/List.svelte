<script>
    import {onMount} from 'svelte';
    import TodoAPI from "../TodoAPI";
    import {items} from '../stores';
    import Item from "./Item.svelte";
    import NewItem from "./NewItem.svelte";
    import { v4 as uuidv4 } from 'uuid';

    function handleNewItem(e){
        $items = [
            {
                id: uuidv4(),
                text: e.detail,
                completed: false
            },
            ...$items
        ];

        TodoAPI.save($items);
    }

    function handleUpdate(e){
        const index = $items.findIndex(item => item.id === e.detail.id);
        $items[index] = e.detail;
        TodoAPI.save($items);
    }

    function handleDelete(e){
        $items = $items.filter(item => item.id !== e.detail)
        TodoAPI.save($items)
    }

    let itemSorted = [];

    $: {
        itemSorted = [...$items];
        itemSorted.sort((a,b) => {
            if (a.completed && b.completed) return 0;
            if (a.completed) return 1;
            if (b.completed) return -1;
        });
    }

    onMount(async () => {
       $items = await TodoAPI.getAll();
       // $items = [id=1, text='Learn about Svelte', completed=false]

    });
</script>

<style>
    .list{
        padding: 15px;

    }
    .list-status{
        margin: 0;
        text-align: center;
        color: #f4f4f4;
        font-weight: bold;
        font-size: 1.1em;
    }
</style>

<div class="list">
    <NewItem on:newitem={handleNewItem}/>
    {#each itemSorted as item (item)}
        <Item {...item} on:update={handleUpdate} on:delete={handleDelete} />
    {:else}
        <p class="list-status">No Items Exists</p>
    {/each}
</div>