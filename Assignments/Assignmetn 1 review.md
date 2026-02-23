Assignmetn 1 review



read

|  Code 1    | Code 2     |
| ---- | ---- |
| \`const result = await sql`select * from todos where id = ${id}`;<br/>		<br/>		if (result.length === 0) <br/>		{<br/>			return null;<br/>		}<br/>		<br/>		const todoData = result[0];<br/>		const todoProps: TodoProps = {<br/>			id: todoData.id,<br/>			title: todoData.title,<br/>			description: todoData.description,<br/>			status: todoData.status,<br/>			dueAt: todoData.due_at,<br/>			createdAt: todoData.created_at,<br/>			completedAt: todoData.completed_at,<br/>			editedAt: todoData.edited_at,<br/>		}<br/>		<br/>		const todo = new Todo(sql, todoProps);<br/>		return todo;`|const [row] = await sql`
    	select * from  todos where id = ${id}
    	`;
    	// Convert snake_case keys → camelCase keys
    	const camelRow = Object.fromEntries(
    		Object.entries(row).map(([key, value]) => [
    			snakeToCamel(key),
    			value,
    		]),
    	);
    
    	if (!row) return null;
    	return new Todo(sql, camelRow as TodoProps);|
