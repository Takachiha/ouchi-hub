<script>
  import { fade } from 'svelte/transition';
  import { push } from 'svelte-spa-router';
  import { showDrawer } from '~/stores.js';
  let isDisabled;

  const openDrawer = () => {
    isDisabled = false;
  };
  const closeDrawer = () => {
    isDisabled = false;
    $showDrawer = false;
  };

  const updateWorks = () => {
    fetch(`http://${API_ADDRESS}/api/v1/works`, {
      method: 'PUT',
      mode: 'cors',
    });
  };
</script>

{#if $showDrawer}
  <div class="drawer {isDisabled ? 'is-disabled' : showDrawer ? 'is-opened' : 'is-closed'}">
    <div class="drawer-header">
      <img src="images/close.png" alt="" class="icon" on:click="{closeDrawer}" />
      <div class="display-name">Ouchi Hub</div>
    </div>
    <div
      class="nav-wrapper"
      on:click="{() => {
        push('/library');
        closeDrawer();
      }}"
    >
      <img src="images/book.png" alt="" class="icon" />
      <div class="nav">ライブラリ</div>
    </div>
    <div
      class="nav-wrapper"
      on:click="{() => {
        updateWorks();
        location.reload();
        closeDrawer();
      }}"
    >
      <img src="images/reload.png" alt="" class="icon" />
      <div class="nav">作品の同期</div>
    </div>
    <div
      class="nav-wrapper"
      on:click="{() => {
        push('/setting/image');
        closeDrawer();
      }}"
    >
      <img src="images/image.png" alt="" class="icon" />
      <div class="nav">作品画像の設定</div>
    </div>
  </div>
  <div class="overlay" on:click="{closeDrawer}" transition:fade="{{ duration: 200 }}"></div>
{/if}

<style>
  .drawer {
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0;
    width: 250px;
    height: 100%;
    background-color: #181818;
    z-index: 1100;
  }

  .drawer-header {
    display: flex;
    justify-content: start;
    align-items: center;
    background: orange;
    width: 250px;
    height: 40px;
  }

  .display-name {
    font-weight: bold;
    color: #ffffff;
    margin-left: 10px;
  }

  @keyframes close-drawer {
    from {
      transform: translateX(0px);
    }
    to {
      transform: translateX(-251px);
    }
  }

  .is-disabled {
    display: none;
  }

  .is-closed {
    display: flex;
    animation: 200ms linear close-drawer 1 forwards;
  }

  .is-opened {
    display: flex;
  }

  .icon {
    width: 40px;
    height: 40px;
    cursor: pointer;
  }

  .nav-wrapper {
    display: flex;
    justify-content: start;
    align-items: center;
    margin: 10px;
    padding-bottom: 5px;
    cursor: pointer;
    color: #ffffff;
    border-bottom: 1px solid #aaaaaa;
  }

  .nav {
    flex-grow: 1;
    height: 40px;
    width: auto;
    font-weight: bold;
    margin-left: 30px;
    margin-top: 13px;
    z-index: 1150;
  }

  .overlay {
    position: fixed;
    width: 100%;
    height: 100%;
    background-color: rgba(50, 50, 50, 0.5);
    z-index: 1050;
  }
</style>
