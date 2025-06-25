<script lang="ts">
	export let setProfileImageUrl: (url: string) => void;

	interface ImageOption {
		id: string;
		src: string;
		alt: string;
	}

	interface Category {
		name: string;
		id: string;
		images: ImageOption[];
	}

	// Only really for styling, indicating selection
	let selectedId: string = '';

	const categories: Category[] = [
		{
			id: 'personal',
			name: 'Personal',
			images: [
				{
					id: 'personal1',
					src: '/assets/images/personal1.png',
					alt: 'Personlig ikon 1'
				},
				{
					id: 'personal2',
					src: '/assets/images/personal2.png',
					alt: 'Personlig ikon 2'
				},
				{
					id: 'personal3',
					src: '/assets/images/personal3.png',
					alt: 'Personlig ikon 3'
				}
			]
		},
		{
			id: 'assistant',
			name: 'Assistant',
			images: [
				{
					id: 'assistant1',
					src: '/assets/images/assistant1.png',
					alt: 'Assistent ikon 1'
				},
				{
					id: 'assistant2',
					src: '/assets/images/assistant2.png',
					alt: 'Assistent ikon 2'
				},
				{
					id: 'assistant3',
					src: '/assets/images/assistant3.png',
					alt: 'Assistent ikon 3'
				}
			]
		},
		{
			id: 'specialist',
			name: 'Specialist',
			images: [
				{
					id: 'specialist1',
					src: '/assets/images/specialist1.png',
					alt: 'Specialist ikon 1'
				},
				{
					id: 'specialist2',
					src: '/assets/images/specialist2.png',
					alt: 'Specialist ikon 2'
				},
				{
					id: 'specialist3',
					src: '/assets/images/specialist3.png',
					alt: 'Specialist ikon 3'
				}
			]
		}
	];

	function imageToBase64(id: string): string | null {
		const img = document.getElementById(id) as HTMLImageElement;
		const canvas = document.createElement('canvas');
		const size = 250;
		canvas.width = size;
		canvas.height = size;
		const ctx = canvas.getContext('2d');

		if (ctx === null || img === null) {
			return null;
		}

		ctx.clearRect(0, 0, size, size);
		ctx.drawImage(img, 0, 0, size, size);
		return canvas.toDataURL('image/png');
	}

	function setImage(id: string) {
		selectedId = id;

		let image = categories
			.flatMap(({ images }) => images)
			.find(({ id: imageId }) => id === imageId);

		if (image) {
			const saveImage = imageToBase64(image.id);

			if (saveImage) {
				setProfileImageUrl(saveImage);
			}
		}
	}
</script>

<hr class="border-gray-100 dark:border-gray-850 my-1.5" />

<h2 class="text-lg font-semibold mb-4">Modellens ikon</h2>

{#each categories as { images, name, id: categoryId }}
	<section class="flex mb-2">
		<h3 class="font-medium mr-4 w-32">{name}</h3>
		{#each images as { id: imageId, src, alt }}
			<button
				type="button"
				class="h-8 w-8 p-0 mr-2 {selectedId === imageId && categoryId === 'personal'
					? 'outline-2 outline-blue-300'
					: ''} {selectedId === imageId && categoryId === 'assistant'
					? 'outline-2 outline-green-300'
					: ''} {selectedId === imageId && categoryId === 'specialist'
					? 'outline-2 outline-purple-300'
					: ''}"
				aria-label={`Vælg ${name}ikonet`}
				on:click={() => setImage(imageId)}
			>
				<img id={imageId} {src} {alt} class="h-8 w-8 object-contain" />
			</button>
		{/each}
	</section>
{/each}

<hr class="border-gray-100 dark:border-gray-850 my-1.5" />
