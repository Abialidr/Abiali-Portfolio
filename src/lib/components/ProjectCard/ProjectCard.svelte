<script lang="ts">
	import { countMonths, getMonthName, getTimeDiff } from '$lib/utils/helpers';
	import Chip from '../Chip/Chip.svelte';
	import Card from '../Card/Card.svelte';
	import CardTitle from '../Card/CardTitle.svelte';
	import CardLink from '../Card/CardLink.svelte';
	import CardDivider from '../Card/CardDivider.svelte';
	import ChipIcon from '../Chip/ChipIcon.svelte';
	import CardLogo from '../Card/CardLogo.svelte';
	import type { Project } from '$lib/types';
	import { getAssetURL } from '$lib/data/assets';
	import { base } from '$app/paths';

	export let project: any;
	export let fromto: Boolean;
	$: months = countMonths(project.period.from, project.period.to);
	// $: period = `${months} month${months > 1 ? 's' : ''}`;
	$: period = `${getTimeDiff(
		project.period.from,
		project.period.to ?? new Date(Date.now() + 1000 * 60 * 60 * 24)
	)}`;
	$: from = `${getMonthName(project.period.from.getMonth())} ${project.period.from.getFullYear()}`;
	$: to = project.period.to
		? `${getMonthName(project.period.to.getMonth())} ${project.period.to.getFullYear()}`
		: 'now';
</script>

<Card color={project.color} href={`${base}/projects/${project.slug}`}>
	<CardLogo alt={project.name} src={project.logo} size={40} radius={'0'} />
	<div class="m-t-20px row justify-between items-center">
		<CardTitle title={project.name} />
		<div class="row">
			{#each project.links as link}
				<CardLink label={link.label ?? ''} to={link.to} />
			{/each}
		</div>
	</div>
	<CardDivider />
	<div
		class="row m-b-15px justify-between text-[var(--secondary-text)] text-0.9em font-italic font-300"
	>
		<p>{project.type}</p>
		{#if fromto}
			<p>{period}</p>
		{/if}
	</div>
	<p class="text-[0.95em] text-[var(--secondary-text)] font-300 m-t-20px m-b-40px flex-1">
		{project.shortDescription}
	</p>
	<div class="row justify-between text-0.8em font-400">
		{#if fromto}
			<Chip>{from}</Chip>
			{#if from !== to}
				<Chip>{to}</Chip>
			{/if}
		{/if}
	</div>
	<CardDivider />
	<div class="row hello">
		{#each project.skills as tech, index}
			{#if index < 6}
				<ChipIcon logo={getAssetURL(tech.logo)} name={tech.name} />
			{/if}
		{/each}
	</div>
</Card>

<style>
</style>
