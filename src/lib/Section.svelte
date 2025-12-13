<script>
  import { createEventDispatcher } from 'svelte';

  export let section;

  const dispatch = createEventDispatcher();

  function updateSectionName(event) {
    dispatch('update', {
      sectionId: section.id,
      field: 'name',
      value: event.target.value
    });
  }

  function addTask() {
    const updatedTasks = [...section.tasks, { name: '', id: '', startDate: '', dependencies: '', duration: '' }];
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
              on:change={(e) => updateTask(index, 'name', e.target.value)}
              placeholder="任务名称"
            />
          </td>
          <td>
            <input
              type="text"
              value={task.id}
              on:change={(e) => updateTask(index, 'id', e.target.value)}
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
            <input
              type="text"
              value={task.dependencies}
              on:change={(e) => updateTask(index, 'dependencies', e.target.value)}
              placeholder="依赖ID"
            />
          </td>
          <td>
            <input
              type="number"
              value={task.duration}
              on:change={(e) => updateTask(index, 'duration', e.target.value)}
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
  }

  .section-name-input {
    font-size: 16px;
    font-weight: bold;
    padding: 5px 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    width: 200px;
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
  input[type="number"] {
    width: 100%;
    padding: 5px;
    border: 1px solid #ddd;
    border-radius: 3px;
    font-size: 13px;
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
    padding: 4px 8px;
    font-size: 12px;
  }

  .button-group {
    display: flex;
    gap: 10px;
  }
</style>
