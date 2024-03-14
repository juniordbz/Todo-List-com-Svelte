<script lang="ts">
  import Card from "$lib/components/Card.svelte";
  import Container from "$lib/components/Container.svelte";
	import Title from "$lib/components/Title.svelte";
  import Icon from '@iconify/svelte';
	import { fade, fly } from "svelte/transition";

  type TaskProps ={
    id: string
    content: string
    checked: boolean
  } 

  let taskList: TaskProps[] = []


 
  const newTask = () => {
    if(!content) return
    taskList = [{id: crypto.randomUUID(), content, checked: false}, ...taskList]
    content = ''
  }
  let content = ''

</script>

<svelte:head>
  <title>Todo-List</title>
</svelte:head>


<Container>
  <Card>
    <Title>Minha lista</Title>
    <hr class="my-3 "/>
    <div class="flex items-center gap-3">
      <input type="text" class="w-full border rounded-full px-4 py-2 outline-none" bind:value={content}>
      <button on:click={newTask}>
        <Icon icon='ep:circle-plus' class='text-3xl text-indigo-400 duration-150 hover:opacity-70'/>
      </button>
    </div>
    <div class="flex flex-col gap-3 pt-4" >
      {#if !taskList.length}
      <div 
        class="text-center text-sm text-slate-600 w-full"
        in:fly={{x:100 , duration:400, delay: 400}}
        out:fly={{x:100 , duration:400 }}
      >
        Adicione uma nova tarefa!
      </div>
      {/if}
      {#each taskList as taskItem (taskItem.id) }
        <div 
          class=" flex w-full items-center justify-between border-b px-2 py-1 hover:bg-slate-50 duration-150"
          in:fly={{x:100 , duration:400, delay: 400}}
          out:fly={{x:100 , duration:400 }}
        >
          <div class="text-sm font-light text-slate-500" class:line-through={taskItem.checked}> {taskItem.content}</div>
          <div class="flex">
            {#if taskItem.checked}
              <button type="button" in:fade on:click={() => {
                taskList = taskList.map(task =>{
                  if(taskItem.id === task.id) task.checked = false
                  return task
                })
              }}>
                <Icon icon='material-symbols:keyboard-return-rounded' class='text-xl text-slate-400 hover:opacity-80 duration-150'/>
              </button>
            {:else}
              <button type="button" in:fade on:click={() => {
                taskList = taskList.map(task =>{
                  if(taskItem.id === task.id) task.checked = true
                  return task
                })
              }}>
                <Icon icon='material-symbols:check-small' class='text-xl text-emerald-400 hover:opacity-80 duration-150'/>
              </button>
            {/if}
              <button type="button" on:click={() => {
                taskList = taskList.filter(task => task.id !== taskItem.id);
              }}> 
                <Icon icon='ic:outline-remove' class='text-xl text-rose-400 hover:opacity-80 duration-150'/>
            </button>
          </div>
        </div>
      {/each}
    </div>
  </Card>
</Container>