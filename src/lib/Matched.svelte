<script>
	export let match;
</script>

<div class="breadcrumb">
	<div class="breadcrumb-item from">
		<span class="breadcrumb__inner">{match.from}</span>
	</div>
	<div class="breadcrumb-item to">
		<span class="breadcrumb__inner">{match.to}</span>
	</div>
</div>

<style>
	.breadcrumb {
		display: flex;
		border-radius: 0.5rem;
		overflow: hidden;
		height: 2.5rem;
	}

	.breadcrumb-item {
    user-select: none;
		position: relative;
		display: flex;
		text-decoration: none;
		padding-left: 2.5rem;
	}

	.breadcrumb__inner {
		z-index: 2;
		line-height: 2.5rem;
		font-size: 1rem;
		font-weight: 600;
	}

	.breadcrumb-item.from {
		background-color: #161853;
		color: #fff;
		padding-right: 1rem;
	}

	.breadcrumb-item.from:after {
		background-color: #161853;
		color: #fff;
	}

	.breadcrumb-item.to {
		background-color: #ec255a;
		color: #fff;
	}

	.breadcrumb .breadcrumb-item:first-child {
		padding-left: 1rem;
	}

	.breadcrumb .breadcrumb-item:last-child {
		padding-right: 1rem;
	}

	.breadcrumb .breadcrumb-item:after {
		content: '';
		position: absolute;
		display: inline-block;
		width: 2.5rem;
		height: 2.5rem;
		top: 0;
		right: -1.25rem;
		border-top-right-radius: 5px;
		transform: scale(0.707) rotate(45deg);
		z-index: 1;
	}

	.breadcrumb .breadcrumb-item:last-child:after {
		content: none;
	}
</style>
