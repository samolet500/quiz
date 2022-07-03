<template>
	<div class="header-nav">
		<div class="container">
			<div class="header-nav__wrap">
				<router-link to="/" class="header-nav__logo">
					<img src="../assets/logo.svg" alt="Vue logo" />
				</router-link>
				
				<div class="header-nav__menu">
					<router-link to="/" class="header-nav__nav-link">Home</router-link>
					<router-link to="/catalog" class="header-nav__nav-link">Catalog</router-link>
					<router-link to="/tenders" class="header-nav__nav-link">Tenders</router-link>
					<router-link to="/trade" class="header-nav__nav-link">Trade</router-link>
					<router-link to="/web-shop-nl" class="header-nav__nav-link">Web-Shop NL</router-link>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "HeaderNav",
};
</script>

<style lang="scss">
.header-nav {
	padding: 8px 0;
	background: #c4c4c4;

	&__wrap {
		display: flex;
		align-items: center;	
	}

	&__menu {
		display: flex;
		align-items: center;
		margin-left: 95px;
	}

	&__nav-link {
		margin-right: 38px;
		font-weight: 400;
		font-size: 16px;
		line-height: 21px;
		text-decoration: none;
		color: #000000;

		&:hover {
			color: #363636;
		}

		&:last-child {
			margin-right: 38px;
		}
	}
}
</style>