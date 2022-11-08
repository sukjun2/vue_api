<template>
  <div class="about">
    <header id="header">
      <div class="header__left">
        <ul>
          <li>
            <router-Link to="/about">About</router-Link>
            <router-Link to="/reference">Reference</router-Link>
          </li>
        </ul>
      </div>
      <h1 class="header__center">
        <router-Link to="/">API site</router-Link>
      </h1>
      <div class="header__right">
        <ul>
          <li>
            <router-Link to="/youtube">Youtube</router-Link>
          </li>
          <li>
            <router-Link to="/movie">Movie</router-Link>
          </li>
          <li>
            <router-Link to="/unsplash">Unsplash</router-Link>
          </li>
        </ul>
      </div>
    </header>
  </div>
</template>

<style lang="scss">
#header {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  background: var(--bg-light);
  border-bottom: 1px solid var(--bg-dark-border);
  display: flex;
  align-items: center;
  color: var(--black);
  padding: 0 10px;
  z-index: 10000;
  a {
    display: inline-block;
    color: var(--black);
    margin: 10px;
    position: relative;
    &::before {
      content: "";
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 1px;
      transform: scaleX(0);
      background: var(--black);
      transition: all 0.3s ease;
    }
    &:hover::before {
      transform: scaleX(1);
    }
  }
  li {
    display: inline-block;
  }
  .header__left {
    flex: 1 1 40%;
    font-family: var(--font-sub2);
  }
  .header__center {
    flex: 1 1 20%;
    text-align: center;
    font-size: 30px;
    font-family: var(--font-sub2);
  }
  .header__right {
    flex: 1 1 40%;
    text-align: right;
    font-family: var(--font-sub2);
  }
}
</style>
