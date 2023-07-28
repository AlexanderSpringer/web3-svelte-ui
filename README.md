
<p>Front-End Metamask authentication in svelte with ethers.js. Backend (required): https://github.com/AlexanderSpringer/web3-nest-postgres-auth</p><br />
<p><b>Live Demo</b>: https://web3-svelte-ui.vercel.app/</p><br />
<p>
Since this is a simple demo some functionality is not yet included:<br />
<ul>
  <li>Cookies not implemented client-side</li>
  <li>Email / Password authentication
    <ul>
      <li>Basically implemented in Nest, but not client-side</li>
      <li>Passwords are stored in clear-text</li>
    </ul>
  </li>
  <li>A logged in user can do all REST calls (no permission levels, like "admin", "user")</li>
</ul>
</p>
<br /><br /><br /><br /><br /><br />


# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/master/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
