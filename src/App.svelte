<script>
  import { onMount } from 'svelte';
  import mermaid from 'mermaid';
  import Section from './lib/Section.svelte';
  import Notification from './lib/Notification.svelte';

  let sections = [];
  let sectionCounter = 0;
  let mermaidCode = '';
  let notification = { show: false, message: '', isError: false };

  onMount(() => {
    mermaid.initialize({
      startOnLoad: false,
      theme: 'default',
      gantt: {
        titleTopMargin: 25,
        barHeight: 20,
        barGap: 4,
        topPadding: 50,
        leftPadding: 75,
        gridLineStartPadding: 35,
        fontSize: 11,
        numberSectionStyles: 4
      }
    });

    // Initialize with example data
    sections = [
      {
        id: sectionCounter++,
        name: '项目规划',
        tasks: [
          { name: '需求分析', id: 'task1', startDate: '2024-01-01', dependencies: '', duration: '5' },
          { name: '设计方案', id: 'task2', startDate: '', dependencies: 'task1', duration: '3' }
        ]
      },
      {
        id: sectionCounter++,
        name: '开发阶段',
        tasks: [
          { name: '前端开发', id: 'task3', startDate: '', dependencies: 'task2', duration: '7' },
          { name: '后端开发', id: 'task4', startDate: '', dependencies: 'task2', duration: '7' }
        ]
      }
    ];

    renderGantt();
  });

  function addSection() {
    const newId = sectionCounter++;
    sections = [
      ...sections,
      {
        id: newId,
        name: `Section ${newId + 1}`,
        tasks: [{ name: '', id: '', startDate: '', dependencies: '', duration: '' }]
      }
    ];
    renderGantt();
  }

  function removeSection(sectionId) {
    sections = sections.filter(s => s.id !== sectionId);
    renderGantt();
  }

  function updateSection(event) {
    const { sectionId, field, value } = event.detail;
    const section = sections.find(s => s.id === sectionId);
    if (section) {
      section[field] = value;
      sections = sections;
      renderGantt();
    }
  }

  function updateTasks(event) {
    const { sectionId, tasks } = event.detail;
    const section = sections.find(s => s.id === sectionId);
    if (section) {
      section.tasks = tasks;
      sections = sections;
      renderGantt();
    }
  }

  function generateMermaidCode() {
    let code = 'gantt\n';
    code += '    title 甘特图\n';
    code += '    dateFormat YYYY-MM-DD\n';

    sections.forEach(section => {
      if (section.tasks.some(t => t.name || t.id)) {
        code += `    section ${section.name}\n`;
        
        section.tasks.forEach(task => {
          if (task.name && task.id) {
            let taskLine = `    ${task.name} :`;
            
            if (task.id) {
              taskLine += ` ${task.id},`;
            }
            
            if (task.dependencies) {
              taskLine += ` after ${task.dependencies},`;
            } else if (task.startDate) {
              taskLine += ` ${task.startDate},`;
            }
            
            if (task.duration) {
              taskLine += ` ${task.duration}d`;
            }
            
            code += taskLine + '\n';
          }
        });
      }
    });

    return code;
  }

  async function renderGantt() {
    mermaidCode = generateMermaidCode();
    const outputDiv = document.getElementById('mermaidOutput');
    if (!outputDiv) return;

    outputDiv.innerHTML = '';

    try {
      const { svg } = await mermaid.render('ganttChart', mermaidCode);
      outputDiv.innerHTML = svg;
    } catch (error) {
      outputDiv.innerHTML = `<div style="color: red; padding: 20px;">
        <h3>渲染错误</h3>
        <p>${error.message}</p>
        <p>请检查输入的数据格式是否正确。</p>
      </div>`;
    }
  }

  async function copyCode() {
    if (navigator.clipboard && navigator.clipboard.writeText) {
      try {
        await navigator.clipboard.writeText(mermaidCode);
        showNotification('代码已复制到剪贴板！');
      } catch (err) {
        fallbackCopy();
      }
    } else {
      fallbackCopy();
    }
  }

  function fallbackCopy() {
    const textarea = document.getElementById('mermaidCode');
    textarea.select();
    try {
      document.execCommand('copy');
      showNotification('代码已复制到剪贴板！');
    } catch (err) {
      showNotification('复制失败，请手动复制', true);
    }
  }

  function showNotification(message, isError = false) {
    notification = { show: true, message, isError };
    setTimeout(() => {
      notification = { show: false, message: '', isError: false };
    }, 3000);
  }
</script>

<div class="container">
  <div class="left-panel">
    <div class="header">
      <h1>任务配置</h1>
      <button class="btn-primary add-section-btn" on:click={addSection}>添加 Section</button>
    </div>
    <div class="sections-container">
      {#each sections as section (section.id)}
        <Section
          {section}
          on:remove={() => removeSection(section.id)}
          on:update={updateSection}
          on:updateTasks={updateTasks}
          on:change={renderGantt}
        />
      {/each}
    </div>
  </div>

  <div class="right-panel">
    <div class="header">
      <h1>甘特图预览</h1>
      <button class="btn-secondary" on:click={renderGantt}>刷新图表</button>
    </div>
    <div id="mermaidOutput"></div>
    <div class="code-section">
      <div class="code-header">
        <h2>Mermaid 代码</h2>
        <button class="btn-secondary" on:click={copyCode}>复制代码</button>
      </div>
      <textarea id="mermaidCode" readonly bind:value={mermaidCode}></textarea>
    </div>
  </div>
</div>

{#if notification.show}
  <Notification message={notification.message} isError={notification.isError} />
{/if}

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(body) {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    height: 100vh;
    overflow: hidden;
  }

  .container {
    display: flex;
    height: 100vh;
  }

  .left-panel {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
    background-color: #f5f5f5;
    border-right: 2px solid #ddd;
  }

  .right-panel {
    flex: 1;
    overflow: auto;
    padding: 20px;
    background-color: #ffffff;
    display: flex;
    flex-direction: column;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 2px solid #333;
  }

  h1 {
    font-size: 24px;
    color: #333;
  }

  h2 {
    font-size: 18px;
    color: #333;
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

  .btn-secondary {
    background-color: #2196F3;
    color: white;
  }

  .btn-secondary:hover {
    background-color: #0b7dda;
  }

  #mermaidOutput {
    flex: 1;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 20px;
    background-color: #fafafa;
    overflow: auto;
  }

  .code-section {
    margin-top: 20px;
  }

  .code-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }

  #mermaidCode {
    width: 100%;
    min-height: 150px;
    padding: 10px;
    font-family: 'Courier New', monospace;
    font-size: 13px;
    border: 1px solid #ddd;
    border-radius: 4px;
    background-color: #f9f9f9;
  }
</style>
