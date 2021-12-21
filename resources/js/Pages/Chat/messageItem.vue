<template>
    <p>{{message.user.name}} : {{message.message}}</p>
    <br><br>


</template>

<script>
export default {
    props : ['message']
}
</script>

<style scoped>
p {
	position: relative;
	max-width: 25em;

	background-color: rgba(59, 130, 246, 0.5);
	padding: 1.125em 1.5em;
	font-size: 1.25em;
	border-radius: 1rem;
  box-shadow:	0 0.125rem 0.5rem rgba(0, 0, 0, .3), 0 0.0625rem 0.125rem rgba(0, 0, 0, .2);
}

p::before {
	content: '';
	position: absolute;
	width: 0;
	height: 0;
	bottom: 100%;
	left: 1.5em;
	border: .75rem solid transparent;
	border-top: none;

	border-bottom-color: rgb(20, 13, 13);
	filter: drop-shadow(0 -0.0625rem 0.0625rem rgba(0, 0, 0, .1));
}
</style>
