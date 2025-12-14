<script>
  import { createEventDispatcher } from 'svelte';

  export let section;
  export let allSections = [];
  export let generateTaskId;

  const dispatch = createEventDispatcher();

  // Get all available tasks from all sections
  $: availableTasks = allSections.flatMap(s => 
    s.tasks
      .filter(t => t.id && t.name)
      .map(t => ({ id: t.id, name: t.name, sectionName: s.name }))
  );

  function updateSectionName(event) {
    dispatch('update', {
      sectionId: section.id,
      field: 'name',
      value: event.target.value
    });
  }

  function addTask() {
    const newTaskId = generateTaskId();
    const updatedTasks = [...section.tasks, { name: '', id: newTaskId, startDate: '', dependencies: '', duration: '' }];
    dispatch('updateTasks', { sectionId: section.id, tasks: updatedTasks });
  }

  function removeTask(index) {
    if (section.tasks.length > 1) {
      const updatedTasks = section.tasks.filter((_, i) => i !== index);
      dispatch('updateTasks', { sectionId: section.id, tasks: updatedTasks });
    }
  }

  function updateTask(index, field, value) {
    const updatedTasks = [...section.tasks];
    updatedTasks[index][field] = value;
    dispatch('updateTasks', { sectionId: section.id, tasks: updatedTasks });
    dispatch('change');
  }
</script>

<div class="section">
  <div class="section-header">
    <input
      type="text"
      class="section-name-input"
      value={section.name}
      on:change={updateSectionName}
      placeholder="Section 名称"
    />
    <div class="button-group">
      <button class="btn-primary btn-small" on:click={addTask}>添加任务</button>
      <button class="btn-danger btn-small" on:click={() => dispatch('remove')}>删除 Section</button>
    </div>
  </div>

  <table>
    <thead>
      <tr>
        <th style="width: 20%">任务名称</th>
        <th style="width: 15%">任务 ID</th>
        <th style="width: 15%">起始日期</th>
        <th style="width: 15%">前置任务</th>
        <th style="width: 15%">任务时长</th>
        <th style="width: 10%">操作</th>
      </tr>
    </thead>
    <tbody>
      {#each section.tasks as task, index (index)}
        <tr>
          <td>
            <input
              type="text"
              value={task.name}
              on:input={(e) => updateTask(index, 'name', e.target.value)}
              placeholder="任务名称"
            />
          </td>
          <td>
            <input
              type="text"
              value={task.id}
              on:input={(e) => updateTask(index, 'id', e.target.value)}
              placeholder="task_id"
            />
          </td>
          <td>
            <input
              type="date"
              value={task.startDate}
              on:change={(e) => updateTask(index, 'startDate', e.target.value)}
            />
          </td>
          <td>
            <select
              value={task.dependencies}
              on:change={(e) => updateTask(index, 'dependencies', e.target.value)}
            >
              <option value="">无依赖</option>
              {#each availableTasks as availableTask}
                {#if availableTask.id !== task.id}
                  <option value={availableTask.id}>
                    {availableTask.name} ({availableTask.id}) - {availableTask.sectionName}
                  </option>
                {/if}
              {/each}
            </select>
          </td>
          <td>
            <input
              type="number"
              value={task.duration}
              on:input={(e) => updateTask(index, 'duration', e.target.value)}
              placeholder="天数"
              min="1"
            />
          </td>
          <td>
            <button class="btn-danger btn-small" on:click={() => removeTask(index)}>删除</button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>

<style>
  .section {
    background: white;
    border-radius: 8px;
    padding: 15px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    padding-bottom: 10px;
    border-bottom: 2px solid #eee;
    gap: 20px;
  }

  .section-name-input {
    font-size: 16px;
    font-weight: bold;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    width: 200px;
    height: 36px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 10px;
  }

  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }

  th {
    background-color: #f0f0f0;
    font-weight: bold;
    font-size: 14px;
  }

  input[type="text"],
  input[type="date"],
  input[type="number"],
  select {
    width: 100%;
    padding: 5px 8px;
    border: 1px solid #ddd;
    border-radius: 3px;
    font-size: 13px;
    height: 32px;
    box-sizing: border-box;
  }

  select {
    cursor: pointer;
    background-color: white;
  }

  select:focus {
    outline: none;
    border-color: #2196F3;
  }

  button {
    padding: 8px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.3s;
  }

  .btn-primary {
    background-color: #4CAF50;
    color: white;
  }

  .btn-primary:hover {
    background-color: #45a049;
  }

  .btn-danger {
    background-color: #f44336;
    color: white;
  }

  .btn-danger:hover {
    background-color: #da190b;
  }

  .btn-small {
    padding: 8px 16px;
    font-size: 12px;
    height: 36px;
    min-width: 80px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }

  .button-group {
    display: flex;
    gap: 10px;
  }
</style>
