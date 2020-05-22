<template>
	<div>
		<div class="header">
			<h1>{{ msg }}</h1>
			<p>change with page</p>
		</div>
		
		<div class="topline">
			<a href="#">link</a>
			<a href="#">link</a>
			<a href="#">link</a>
		</div>
		
		<div class="row">
			<div class="column">
				<h2>First</h2>
			</div>
			<div class="column">
				<h2>Second</h2>
			</div>
			<div class="column">
				<h2>Third</h2>
			</div>
		</div>
		<div class="side">
			
		</div>
	</div>
</template>

<script>
	export default {
		name:'HomePage',
		props:{
			msg: String
		}
	};
</script>

<style>
	/* header */
	.header {
		background-color: cornflowerblue;
		padding: 20px;
		text-align: center;
	}
	
	/* topline */
	.topline {
		background-color: darkblue;
		overflow: hidden;
	}
	
	/* topline link */
	.topline a {
		float: left;
		display: block;
		color: burlywood;
		text-align: center;
		padding: 14px 16px;
		text-decoration: none;
	}
	
	
	.column {
		float: left;
		width: 33.33%;
	}
	
	.row:after {
		content: "";
		display: table;
		clear: both;
	}
	
	@media screen and (max-width:600px) {
		.column {
			width: 100%;
		}
	}
</style>
