<script lang="ts">
	import { Loader2 } from 'lucide-svelte';
	import { zodClient } from 'sveltekit-superforms/adapters';
	import * as Form from '$lib/components/ui/form';
	import { Input } from '$lib/components/ui/input';
	import { superForm, type SuperValidated, type Infer } from 'sveltekit-superforms';

	import { loginSchema, type LoginSchema } from '$lib/schemas.js';
	import Separator from '@/components/ui/separator/separator.svelte';

	export let data: SuperValidated<Infer<LoginSchema>>;

	const form = superForm(data, {
		validators: zodClient(loginSchema),
		dataType: 'json'
	});

	const formData = form.form;
</script>

<div class="">
	<h1 class="py-5 text-center text-2xl font-semibold">Welcome</h1>

	<form method="POST" action="/login" use:form.enhance>
		<Form.Field {form} name="email">
			<Form.Control let:attrs>
				<Form.Label>Username</Form.Label>
				<Input {...attrs} bind:value={$formData.email} />
			</Form.Control>
			<Form.FieldErrors />
		</Form.Field>
		<Form.Field {form} name="password">
			<Form.Control let:attrs>
				<Form.Label>Password</Form.Label>
				<Input type="password" {...attrs} bind:value={$formData.password} />
			</Form.Control>
			<Form.FieldErrors />
		</Form.Field>
		<Form.Button class="w-full">
			{#if !form.delayed}
				<Loader2 class="size-6 animate-spin" />
			{:else}
				Login
			{/if}
		</Form.Button>

		<div class="my-4 flex items-center justify-center">
			<Separator class="flex-1" />
			<div class="mx-4 text-gray-500">OR</div>
			<Separator class="flex-1" />
		</div>

		<Form.Button variant="secondary" href="/register" class="w-full">Register</Form.Button>
	</form>
</div>
