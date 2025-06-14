<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Task Manager</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 2rem;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 2rem;
            font-size: 2.5rem;
            background: linear-gradient(45deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .input-section {
            display: flex;
            gap: 10px;
            margin-bottom: 2rem;
        }

        #prioritySelect {
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            outline: none;
            background: white;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 140px;
        }

        #prioritySelect:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        #taskInput {
            flex: 1;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            outline: none;
            transition: all 0.3s ease;
        }

        #taskInput:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        #addBtn {
            padding: 15px 25px;
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            min-width: 80px;
        }

        #addBtn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        #addBtn:active {
            transform: translateY(0);
        }

        .stats {
            display: flex;
            justify-content: space-between;
            margin-bottom: 1.5rem;
            padding: 15px;
            background: rgba(102, 126, 234, 0.1);
            border-radius: 10px;
        }

        .stat-item {
            text-align: center;
        }

        .stat-number {
            font-size: 1.5rem;
            font-weight: bold;
            color: #667eea;
        }

        .stat-label {
            font-size: 0.9rem;
            color: #666;
            margin-top: 5px;
        }

        #taskList {
            list-style: none;
            max-height: 400px;
            overflow-y: auto;
        }

        .task-item {
            background: white;
            margin-bottom: 10px;
            padding: 15px;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            animation: taskSlideIn 0.3s ease-out;
            border-left: 4px solid transparent;
        }

        .task-item.priority-high {
            border-left-color: #ff4757;
            background: linear-gradient(90deg, rgba(255, 71, 87, 0.1) 0%, white 10%);
        }

        .task-item.priority-medium {
            border-left-color: #ffa502;
            background: linear-gradient(90deg, rgba(255, 165, 2, 0.1) 0%, white 10%);
        }

        .task-item.priority-low {
            border-left-color: #2ed573;
            background: linear-gradient(90deg, rgba(46, 213, 115, 0.1) 0%, white 10%);
        }

        @keyframes taskSlideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .task-item:hover {
            transform: translateX(5px);
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
        }

        .task-item.completed {
            opacity: 0.7;
            background: #f8f9fa;
        }

        .task-content {
            display: flex;
            align-items: center;
            flex: 1;
        }

        .task-checkbox {
            width: 20px;
            height: 20px;
            margin-right: 15px;
            cursor: pointer;
            accent-color: #667eea;
        }

        .task-text {
            flex: 1;
            font-size: 16px;
            color: #333;
            transition: all 0.3s ease;
        }

        .task-text.completed {
            text-decoration: line-through;
            color: #999;
        }

        .task-actions {
            display: flex;
            gap: 5px;
            align-items: center;
        }

        .priority-badge {
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 12px;
            font-weight: bold;
            text-transform: uppercase;
            margin-right: 8px;
        }

        .priority-high {
            background: rgba(255, 71, 87, 0.2);
            color: #ff4757;
        }

        .priority-medium {
            background: rgba(255, 165, 2, 0.2);
            color: #ffa502;
        }

        .priority-low {
            background: rgba(46, 213, 115, 0.2);
            color: #2ed573;
        }

        .edit-btn {
            background: #3742fa;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
            margin-right: 5px;
        }

        .edit-btn:hover {
            background: #2f3542;
            transform: scale(1.05);
        }

        .task-date {
            font-size: 12px;
            color: #999;
            margin-top: 5px;
        }

        .search-section {
            margin-bottom: 1rem;
        }

        #searchInput {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            outline: none;
            transition: all 0.3s ease;
        }

        #searchInput:focus {
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .clear-completed-btn {
            background: #ff6b6b;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
            margin-top: 1rem;
            width: 100%;
        }

        .clear-completed-btn:hover {
            background: #ff5252;
            transform: translateY(-2px);
        }

        .clear-completed-btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        .delete-btn {
            background: #ff4757;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .delete-btn:hover {
            background: #ff3742;
            transform: scale(1.05);
        }

        .empty-state {
            text-align: center;
            padding: 3rem 2rem;
            color: #666;
        }

        .empty-icon {
            font-size: 4rem;
            margin-bottom: 1rem;
            opacity: 0.5;
        }

        .empty-text {
            font-size: 1.2rem;
            margin-bottom: 0.5rem;
        }

        .empty-subtext {
            font-size: 1rem;
            opacity: 0.7;
        }

        .filter-section {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 1.5rem;
        }

        .filter-btn {
            padding: 8px 16px;
            border: 2px solid #e0e0e0;
            background: white;
            border-radius: 20px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .filter-btn.active,
        .filter-btn:hover {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        @media (max-width: 480px) {
            .container {
                padding: 1.5rem;
                margin: 10px;
            }

            h1 {
                font-size: 2rem;
            }

            .input-section {
                flex-direction: column;
            }

            .stats {
                flex-direction: column;
                gap: 10px;
            }

            .filter-section {
                flex-wrap: wrap;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Smart Task Manager</h1>
        
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a new task..." maxlength="100">
            <select id="prioritySelect">
                <option value="low">Low Priority</option>
                <option value="medium" selected>Medium Priority</option>
                <option value="high">High Priority</option>
            </select>
            <button id="addBtn">Add Task</button>
        </div>

        <div class="stats">
            <div class="stat-item">
                <div class="stat-number" id="totalTasks">0</div>
                <div class="stat-label">Total</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="completedTasks">0</div>
                <div class="stat-label">Completed</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="pendingTasks">0</div>
                <div class="stat-label">Pending</div>
            </div>
        </div>

        <div class="search-section">
            <input type="text" id="searchInput" placeholder="Search tasks...">
        </div>

        <div class="filter-section">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="pending">Pending</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
            <button class="filter-btn" data-filter="high">High Priority</button>
        </div>

        <ul id="taskList"></ul>

        <button class="clear-completed-btn" id="clearCompletedBtn" disabled>Clear Completed Tasks</button>

        <div class="empty-state" id="emptyState" style="display: none;">
            <div class="empty-icon">📝</div>
            <div class="empty-text">No tasks yet!</div>
            <div class="empty-subtext">Add your first task to get started</div>
        </div>
    </div>

    <script>
        class TaskManager {
            constructor() {
                this.tasks = [];
                this.currentFilter = 'all';
                this.searchQuery = '';
                this.taskIdCounter = 1;
                this.editingTaskId = null;
                this.init();
            }

            init() {
                this.loadTasks();
                this.bindEvents();
                this.render();
            }

            bindEvents() {
                const addBtn = document.getElementById('addBtn');
                const taskInput = document.getElementById('taskInput');
                const searchInput = document.getElementById('searchInput');
                const clearCompletedBtn = document.getElementById('clearCompletedBtn');
                const filterBtns = document.querySelectorAll('.filter-btn');

                addBtn.addEventListener('click', () => this.addTask());
                taskInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') this.addTask();
                });

                searchInput.addEventListener('input', (e) => {
                    this.searchQuery = e.target.value.toLowerCase();
                    this.render();
                });

                clearCompletedBtn.addEventListener('click', () => this.clearCompleted());

                filterBtns.forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        this.setFilter(e.target.dataset.filter);
                    });
                });
            }

            addTask() {
                const input = document.getElementById('taskInput');
                const prioritySelect = document.getElementById('prioritySelect');
                const text = input.value.trim();

                if (text === '') {
                    this.showInputError();
                    return;
                }

                if (this.editingTaskId) {
                    // Update existing task
                    const task = this.tasks.find(t => t.id === this.editingTaskId);
                    if (task) {
                        task.text = text;
                        task.priority = prioritySelect.value;
                        task.updatedAt = new Date().toISOString();
                    }
                    this.editingTaskId = null;
                    document.getElementById('addBtn').textContent = 'Add Task';
                } else {
                    // Create new task
                    const task = {
                        id: this.taskIdCounter++,
                        text: text,
                        priority: prioritySelect.value,
                        completed: false,
                        createdAt: new Date().toISOString(),
                        updatedAt: new Date().toISOString()
                    };
                    this.tasks.unshift(task);
                }

                input.value = '';
                prioritySelect.value = 'medium';
                this.saveTasks();
                this.render();
                this.showSuccessAnimation();
            }

            showInputError() {
                const input = document.getElementById('taskInput');
                input.style.borderColor = '#ff4757';
                input.placeholder = 'Please enter a task!';
                
                setTimeout(() => {
                    input.style.borderColor = '#e0e0e0';
                    input.placeholder = 'Enter a new task...';
                }, 2000);
            }

            showSuccessAnimation() {
                const addBtn = document.getElementById('addBtn');
                addBtn.style.transform = 'scale(0.95)';
                addBtn.style.background = '#2ed573';
                addBtn.textContent = '✓';
                
                setTimeout(() => {
                    addBtn.style.transform = 'scale(1)';
                    addBtn.style.background = 'linear-gradient(45deg, #667eea, #764ba2)';
                    addBtn.textContent = 'Add Task';
                }, 300);
            }

            editTask(id) {
                const task = this.tasks.find(t => t.id === id);
                if (task) {
                    document.getElementById('taskInput').value = task.text;
                    document.getElementById('prioritySelect').value = task.priority;
                    document.getElementById('addBtn').textContent = 'Update Task';
                    this.editingTaskId = id;
                    document.getElementById('taskInput').focus();
                }
            }

            clearCompleted() {
                if (confirm('Are you sure you want to delete all completed tasks?')) {
                    this.tasks = this.tasks.filter(t => !t.completed);
                    this.saveTasks();
                    this.render();
                }
            }

            toggleTask(id) {
                const task = this.tasks.find(t => t.id === id);
                if (task) {
                    task.completed = !task.completed;
                    this.saveTasks();
                    this.render();
                }
            }

            deleteTask(id) {
                const taskElement = document.querySelector(`[data-task-id="${id}"]`);
                if (taskElement) {
                    taskElement.style.animation = 'taskSlideOut 0.3s ease-out forwards';
                    setTimeout(() => {
                        this.tasks = this.tasks.filter(t => t.id !== id);
                        this.saveTasks();
                        this.render();
                    }, 300);
                }
            }

            setFilter(filter) {
                this.currentFilter = filter;
                
                // Update active filter button
                document.querySelectorAll('.filter-btn').forEach(btn => {
                    btn.classList.remove('active');
                });
                document.querySelector(`[data-filter="${filter}"]`).classList.add('active');
                
                this.render();
            }

            getFilteredTasks() {
                let filtered = this.tasks;

                // Apply text search
                if (this.searchQuery) {
                    filtered = filtered.filter(task => 
                        task.text.toLowerCase().includes(this.searchQuery)
                    );
                }

                // Apply status/priority filter
                switch (this.currentFilter) {
                    case 'completed':
                        filtered = filtered.filter(task => task.completed);
                        break;
                    case 'pending':
                        filtered = filtered.filter(task => !task.completed);
                        break;
                    case 'high':
                        filtered = filtered.filter(task => task.priority === 'high');
                        break;
                    default:
                        // 'all' - no additional filtering
                        break;
                }

                // Sort by priority and creation date
                return filtered.sort((a, b) => {
                    if (a.completed !== b.completed) {
                        return a.completed - b.completed; // Pending tasks first
                    }
                    
                    const priorityOrder = { high: 3, medium: 2, low: 1 };
                    if (priorityOrder[a.priority] !== priorityOrder[b.priority]) {
                        return priorityOrder[b.priority] - priorityOrder[a.priority]; // High priority first
                    }
                    
                    return new Date(b.createdAt) - new Date(a.createdAt); // Newest first
                });
            }

            render() {
                const taskList = document.getElementById('taskList');
                const emptyState = document.getElementById('emptyState');
                const filteredTasks = this.getFilteredTasks();

                // Clear current tasks
                taskList.innerHTML = '';

                // Show/hide empty state
                if (filteredTasks.length === 0) {
                    emptyState.style.display = 'block';
                    if (this.currentFilter !== 'all') {
                        emptyState.querySelector('.empty-text').textContent = `No ${this.currentFilter} tasks`;
                        emptyState.querySelector('.empty-subtext').textContent = `Try switching to a different filter`;
                    } else {
                        emptyState.querySelector('.empty-text').textContent = 'No tasks yet!';
                        emptyState.querySelector('.empty-subtext').textContent = 'Add your first task to get started';
                    }
                } else {
                    emptyState.style.display = 'none';
                }

                // Render tasks
                filteredTasks.forEach(task => {
                    const li = document.createElement('li');
                    li.className = `task-item priority-${task.priority} ${task.completed ? 'completed' : ''}`;
                    li.setAttribute('data-task-id', task.id);
                    
                    const dateStr = new Date(task.createdAt).toLocaleDateString('ar-EG', {
                        year: 'numeric',
                        month: 'short',
                        day: 'numeric'
                    });
                    
                    li.innerHTML = `
                        <div class="task-content">
                            <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''}>
                            <div>
                                <span class="task-text ${task.completed ? 'completed' : ''}">${this.escapeHtml(task.text)}</span>
                                <div class="task-date">Created: ${dateStr}</div>
                            </div>
                        </div>
                        <div class="task-actions">
                            <span class="priority-badge priority-${task.priority}">${task.priority}</span>
                            <button class="edit-btn" ${task.completed ? 'disabled' : ''}>Edit</button>
                            <button class="delete-btn">Delete</button>
                        </div>
                    `;

                    // Bind events
                    const checkbox = li.querySelector('.task-checkbox');
                    const editBtn = li.querySelector('.edit-btn');
                    const deleteBtn = li.querySelector('.delete-btn');

                    checkbox.addEventListener('change', () => this.toggleTask(task.id));
                    editBtn.addEventListener('click', () => this.editTask(task.id));
                    deleteBtn.addEventListener('click', () => this.deleteTask(task.id));

                    taskList.appendChild(li);
                });

                this.updateStats();
                
                // Update clear completed button
                const clearBtn = document.getElementById('clearCompletedBtn');
                const hasCompleted = this.tasks.some(t => t.completed);
                clearBtn.disabled = !hasCompleted;
            }

            updateStats() {
                const total = this.tasks.length;
                const completed = this.tasks.filter(t => t.completed).length;
                const pending = total - completed;

                document.getElementById('totalTasks').textContent = total;
                document.getElementById('completedTasks').textContent = completed;
                document.getElementById('pendingTasks').textContent = pending;
            }

            escapeHtml(text) {
                const div = document.createElement('div');
                div.textContent = text;
                return div.innerHTML;
            }

            saveTasks() {
                // In a real environment, uncomment the line below to use localStorage
                // localStorage.setItem('smartTaskManager', JSON.stringify(this.tasks));
                
                // For Claude's environment, we'll just keep tasks in memory
                console.log('Tasks saved to memory:', this.tasks);
            }

            loadTasks() {
                try {
                    // In a real environment, uncomment the lines below to use localStorage
                    // const saved = localStorage.getItem('smartTaskManager');
                    // if (saved) {
                    //     this.tasks = JSON.parse(saved);
                    //     this.taskIdCounter = Math.max(...this.tasks.map(t => t.id), 0) + 1;
                    // }
                    
                    // For demonstration, let's add some sample tasks
                    if (this.tasks.length === 0) {
                        this.tasks = [
                            { id: 1, text: 'Welcome to Smart Task Manager!', priority: 'high', completed: false, createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() },
                            { id: 2, text: 'Try adding your own tasks', priority: 'medium', completed: false, createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() },
                            { id: 3, text: 'Click checkboxes to complete tasks', priority: 'low', completed: true, createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() },
                            { id: 4, text: 'Use search to find specific tasks', priority: 'medium', completed: false, createdAt: new Date().toISOString(), updatedAt: new Date().toISOString() }
                        ];
                        this.taskIdCounter = 5;
                    }
                } catch (error) {
                    console.error('Error loading tasks:', error);
                    this.tasks = [];
                }
            }
        }

        // Add custom CSS animation for task removal
        const style = document.createElement('style');
        style.textContent = `
            @keyframes taskSlideOut {
                from {
                    opacity: 1;
                    transform: translateX(0);
                    max-height: 70px;
                    margin-bottom: 10px;
                }
                to {
                    opacity: 0;
                    transform: translateX(-100%);
                    max-height: 0;
                    margin-bottom: 0;
                    padding: 0;
                }
            }
        `;
        document.head.appendChild(style);

        // Initialize the task manager when the page loads
        document.addEventListener('DOMContentLoaded', () => {
            new TaskManager();
        });
    </script>
</body>
</html>
