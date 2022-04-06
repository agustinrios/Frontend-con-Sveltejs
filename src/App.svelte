<script>
	let todos = []
	let todo = { id: '', texto: '', estado: false}

	if (localStorage.getItem("todos")) {
  		todos = JSON.parse(localStorage.getItem("todos"));
	}

	$: localStorage.setItem("todos", JSON.stringify(todos));

	const addTodo = () => {
		if(!todo.texto.trim()){
			console.log('texto vacio')
			todo.texto = ''
			return
		}
		todo.id = Date.now()
		todos = [...todos, todo]
		console.log(todos)

		todo = { id: '', texto: '', estado: false}
	}

	const delTodo = id => {
		todos = todos.filter(item => item.id !== id)
	}

	const editTodo = id => {
		todos = todos.map(item => 
			item.id === id ? 
			{...item, estado: !item.estado} 
			: item
		)
	}

	$: classEstado = (valor) =>
  		valor ? "btn btn-sm btn-success" : "btn btn-sm btn-warning";
	$: classIcono = (valor) => (valor ? "bi bi-arrow-clockwise" : "bi bi-check2");
</script>

<div class="container">
	<h1 class="display-5 my-3">CRUD</h1>
	<form on:submit|preventDefault={addTodo}>
		<input
			type="text"
			placeholder="enter para agregar"
			class="form-control shadow border-0"
			bind:value={todo.texto}
		/>
	</form>
	{#each todos as item}
		<div class="shadow my-3 p-3 lead">
			<p class={item.estado ? 'text-decoration-line-through' : ''}>{item.texto}</p>
			<button class="btn btn-sm {classEstado(item.estado)}">
				<i class={classIcono(item.estado)} on:click={editTodo(item.id)}></i>
			</button>
			<button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
				<i class="bi bi-trash"></i>
			</button>
		</div>
	{/each}
</div>

