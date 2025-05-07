<script>
	import {v4} from 'uuid';

	let product = {
		id: "",
		name: '',
		description: '',
		imageUrl: '',
		category: ''
	};

	let products = [
		{
			id: 1,
			name: 'Product 1',
			description: 'Description of Product 1',
			imageUrl: 'https://example.com/image1.jpg',
			category: 'electronics'
		},
		{
			id: 2,
			name: 'Product 2',
			description: 'Description of Product 2',
			imageUrl: 'https://example.com/image2.jpg',
			category: 'furniture'
		},
		{
			id: 3,
			name: 'Product 3',
			description: 'Description of Product 3',
			imageUrl: 'https://example.com/image3.jpg',
			category: 'clothing'
		}
	];

	let editMode = false;

	const clearForm = () => {
		product = {
			id: "",
			name: '',
			description: '',
			imageUrl: '',
			category: ''
		};
	};

	const onSubmitHandler = (event) => {
		event.preventDefault();

		if (editMode) {
			products = products.map((p) => {
				if (p.id === product.id) {
					return {
						...p,
						name: product.name,
						description: product.description,
						imageUrl: product.imageUrl,
						category: product.category
					};
				}
				return p;
			});
			editMode = false;
		} else {
			const newProduct = {
				id: v4(),
				name: product.name,
				description: product.description,
				imageUrl: product.imageUrl,
				category: product.category
			};
			products = products.concat(newProduct);
		}

		clearForm();
	};
	
	const deleteProduct = (id) => {
		products = products.filter((product) => product.id !== id);
	}

	const updateProduct = (productEdited) => {
		product = productEdited;
		editMode = true;
		/*products = products.map((p) => {
			if (p.id === id) {
				return {
					...p,
					name: product.name,
					description: product.description,
					imageUrl: product.imageUrl,
					category: product.category
				};
			}
			return p;
		});
		clearForm();
		*/
	}
</script>

<main>
	<div class="container p-4">
		<div class="row">
			<div class="col-md-6">
				{#each products as product}
					<div class="card mt-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageUrl}
									<img src="img/no-product.png" alt="" class="card-img-top p-2" />
								{:else}
									<img src={product.imageUrl}  alt="" class="card-img-top p-2"/>
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5 class="card-title">
										<strong>{product.name}</strong>
										<span>
											<small class="text-muted">ID: {product.category}</small>
										</span>
									</h5>
									<p class="card-text">{product.description}</p>
									<button class="btn btn-danger" on:click={deleteProduct(product.id)}>
										Delete
									</button>
									<button class="btn btn-secondary" on:click={updateProduct(product)}>
										Edit
									</button>

								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>

			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault ={onSubmitHandler}>
							<div class="form-group">
								<input bind:value= {product.name} type="text" placeholder="Product Name" id="product-name" class="form-control" />
							</div>
							<div class="form-group">
								<textarea bind:value= {product.description} id="product-description" placeholder="Product Description" rows="3" class="form-control"></textarea>
							</div>
							<div class="form-group">
								<input bind:value= {product.imageUrl} type="url" id="product-image-url" placeholder="https://faztweb.com/" class="form-control"/>
							</div>
							<div class="form-group">
								<select id="category" bind:value= {product.category} class="form-control">
									<option value="laptop">Electronics</option>
									<option value="pc">Furniture</option>
									<option value="peripheral device">Peripheral devices</option>
									<option value="earphones">Earphones</option>
									<option value="monitor">Monitors</option>
								</select>
							</div>

							<button class="btn btn-primary">
								{#if editMode}
									Edit Product
								{:else}
									Add Product
								{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>

	</div>

</main>

<style>
</style>