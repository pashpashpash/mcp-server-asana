# MCP Server for Asana
A fork of @roychri's MCP (Model Context Protocol) server implementation for Asana, allowing you to interact with the Asana API from MCP clients such as Anthropic's Claude Desktop Application.

More details on MCP here:
 - https://www.anthropic.com/news/model-context-protocol
 - https://modelcontextprotocol.io/introduction
 - https://github.com/modelcontextprotocol

## Usage

In the AI tool of your choice (ex: Claude Desktop) ask something about asana tasks, projects, workspaces, and/or comments. Mentioning the word "asana" will increase the chance of having the LLM pick the right tool.

Example:

> How many unfinished asana tasks do we have in our Sprint 30 project?

Another example:

![Claude Desktop Example](https://raw.githubusercontent.com/pashpashpash/mcp-server-asana/main/mcp-server-asana-claude-example.png)

## Tools

1. `asana_list_workspaces`
    * List all available workspaces in Asana
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of workspaces

2. `asana_search_projects`
    * Search for projects in Asana using name pattern matching
    * Required input:
        * workspace (string): The workspace to search in
        * name_pattern (string): Regular expression pattern to match project names
    * Optional input:
        * archived (boolean): Only return archived projects (default: false)
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of matching projects

3. `asana_search_tasks`
    * Search tasks in a workspace with advanced filtering options
    * Required input:
        * workspace (string): The workspace to search in
    * Optional input:
        * text (string): Text to search for in task names and descriptions
        * resource_subtype (string): Filter by task subtype (e.g. milestone)
        * completed (boolean): Filter for completed tasks
        * is_subtask (boolean): Filter for subtasks
        * has_attachment (boolean): Filter for tasks with attachments
        * is_blocked (boolean): Filter for tasks with incomplete dependencies
        * is_blocking (boolean): Filter for incomplete tasks with dependents
        * assignee, projects, sections, tags, teams, and many other advanced filters
        * sort_by (string): Sort by due_date, created_at, completed_at, likes, modified_at (default: modified_at)
        * sort_ascending (boolean): Sort in ascending order (default: false)
        * opt_fields (string): Comma-separated list of optional fields to include
        * custom_fields (object): Object containing custom field filters
    * Returns: List of matching tasks

4. `asana_get_task`
    * Get detailed information about a specific task
    * Required input:
        * task_id (string): The task ID to retrieve
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Detailed task information

5. `asana_create_task`
    * Create a new task in a project
    * Required input:
        * project_id (string): The project to create the task in
        * name (string): Name of the task
    * Optional input:
        * notes (string): Description of the task
        * html_notes (string): HTML-like formatted description of the task
        * due_on (string): Due date in YYYY-MM-DD format
        * assignee (string): Assignee (can be 'me' or a user ID)
        * followers (array of strings): Array of user IDs to add as followers
        * parent (string): The parent task ID to set this task under
        * projects (array of strings): Array of project IDs to add this task to
    * Returns: Created task information

6. `asana_get_task_stories`
    * Get comments and stories for a specific task
    * Required input:
        * task_id (string): The task ID to get stories for
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of task stories/comments

7. `asana_update_task`
    * Update an existing task's details
    * Required input:
        * task_id (string): The task ID to update
    * Optional input:
        * name (string): New name for the task
        * notes (string): New description for the task
        * due_on (string): New due date in YYYY-MM-DD format
        * assignee (string): New assignee (can be 'me' or a user ID)
        * completed (boolean): Mark task as completed or not
    * Returns: Updated task information

8. `asana_get_project`
    * Get detailed information about a specific project
    * Required input:
        * project_id (string): The project ID to retrieve
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Detailed project information

9. `asana_get_project_task_counts`
    * Get the number of tasks in a project
    * Required input:
        * project_id (string): The project ID to get task counts for
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Task count information

10. `asana_get_project_sections`
    * Get sections in a project
    * Required input:
        * project_id (string): The project ID to get sections for
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of project sections

11. `asana_create_task_story`
    * Create a comment or story on a task
    * Required input:
        * task_id (string): The task ID to add the story to
        * text (string): The text content of the story/comment
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Created story information

12. `asana_add_task_dependencies`
    * Set dependencies for a task
    * Required input:
        * task_id (string): The task ID to add dependencies to
        * dependencies (array of strings): Array of task IDs that this task depends on
    * Returns: Updated task dependencies

13. `asana_add_task_dependents`
    * Set dependents for a task (tasks that depend on this task)
    * Required input:
        * task_id (string): The task ID to add dependents to
        * dependents (array of strings): Array of task IDs that depend on this task
    * Returns: Updated task dependents

14. `asana_create_subtask`
    * Create a new subtask for an existing task
    * Required input:
        * parent_task_id (string): The parent task ID to create the subtask under
        * name (string): Name of the subtask
    * Optional input:
        * notes (string): Description of the subtask
        * due_on (string): Due date in YYYY-MM-DD format
        * assignee (string): Assignee (can be 'me' or a user ID)
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Created subtask information

15. `asana_get_multiple_tasks_by_gid`
    * Get detailed information about multiple tasks by their GIDs (maximum 25 tasks)
    * Required input:
        * task_ids (array of strings or comma-separated string): Task GIDs to retrieve (max 25)
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of detailed task information

16. `asana_get_project_status`
    * Get a project status update
    * Required input:
        * project_status_gid (string): The project status GID to retrieve
    * Optional input:
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Project status information

17. `asana_get_project_statuses`
    * Get all status updates for a project
    * Required input:
        * project_gid (string): The project GID to get statuses for
    * Optional input:
        * limit (number): Results per page (1-100)
        * offset (string): Pagination offset token
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: List of project status updates

18. `asana_create_project_status`
    * Create a new status update for a project
    * Required input:
        * project_gid (string): The project GID to create the status for
        * text (string): The text content of the status update
    * Optional input:
        * color (string): The color of the status (green, yellow, red)
        * title (string): The title of the status update
        * html_text (string): HTML formatted text for the status update
        * opt_fields (string): Comma-separated list of optional fields to include
    * Returns: Created project status information

19. `asana_delete_project_status`
    * Delete a project status update
    * Required input:
        * project_status_gid (string): The project status GID to delete
    * Returns: Deletion confirmation

## Prompts

1. `task-summary`
    * Get a summary and status update for a task based on its notes, custom fields and comments
    * Required input:
        * task_id (string): The task ID to get summary for
    * Returns: A detailed prompt with instructions for generating a task summary

## Setup

### Local Installation

1. Clone the repository:
```bash
git clone https://github.com/pashpashpash/mcp-server-asana.git
cd mcp-server-asana
```

2. Install dependencies:
```bash
npm install
```

3. Build the project:
```bash
npm run build
```

### Alternative Installation

You can also use `npx` to run the server directly (not recommended for development).

### Configuration

1. **Create an Asana account**:
   - Visit [Asana](https://www.asana.com)
   - Click "Sign up"

2. **Retrieve the Asana Access Token**:
   - Generate a personal access token from the [Asana developer console](https://app.asana.com/0/my-apps)
   - More details here: https://developers.asana.com/docs/personal-access-token

3. **Configure Claude Desktop**:
   Add the following to your `claude_desktop_config.json`:

   For local installation:
   ```json
   {
     "mcpServers": {
       "asana": {
         "command": "node",
         "args": ["path/to/build/index.js"],
         "env": {
           "ASANA_ACCESS_TOKEN": "your-asana-access-token"
         }
       }
     }
   }
   ```

Note: Replace "path/to/build/index.js" with the **actual path** to your built index.js file. KEEP IN MIND, by default it will be in `./dist/index.js` according to `tsconfig.json`:

```
{
  "extends": "@tsconfig/node20/tsconfig.json",
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "esModuleInterop": true,
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "skipLibCheck": true
  },
  "ts-node": {
    "esm": true,
    "experimentalSpecifiers": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## Troubleshooting

If you encounter permission errors:

1. Ensure the asana plan you have allows API access
2. Confirm the access token and configuration are correctly set in `claude_desktop_config.json`

## Development

### Testing Locally with the MCP Inspector

To test your changes, you can use the MCP Inspector:

```bash
npm run inspector
```

This will expose the client to port `5173` and server to port `3000`.

If those ports are already used by something else, you can use:

```bash
CLIENT_PORT=5009 SERVER_PORT=3009 npm run inspector
```

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
