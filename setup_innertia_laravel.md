# How to install inertia in old laravel

- Install dependencies
    ```
    composer require inertiajs/inertia-laravel
    ```
- Create app.blade.php file on resource/views/app.blade.php
    ```
    <!DOCTYPE html>
    <html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
        <head>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1">

            <title inertia>{{ config('app.name', 'Laravel') }}</title>

            <!-- Fonts -->
            <link rel="preconnect" href="https://fonts.bunny.net">
            <link href="https://fonts.bunny.net/css?family=figtree:400,500,600&display=swap" rel="stylesheet" />

            <!-- Scripts -->
            @routes
            @viteReactRefresh
            @vite(['resources/js/app.jsx', "resources/js/Pages/{$page['component']}.jsx"])
            @inertiaHead
        </head>
        <body class="font-sans antialiased">
            @inertia
        </body>
    </html>
    ```
- Create middleware and add this to global web route array in app/Http/Kernel.php

    ```
    php artisan inertia:middleware
    ```
- Install ziggy both for frontend and backend
    ```
    npm install ziggy-js
    ```

    ```
    composer require tightenco/ziggy
    ```
- Setup Vite.config.js
    ```
    import { defineConfig } from 'vite';
    import laravel from 'laravel-vite-plugin';
    import vue from '@vitejs/plugin-vue';
    import react from '@vitejs/plugin-react';

    export default defineConfig({
        plugins: [
            laravel({
                input: 'resources/js/app.jsx',
                ssr: 'resources/js/ssr.jsx',
                refresh: true,
            }),
            react(),
            // vue({
            //     template: {
            //         transformAssetUrls: {
            //             base: null,
            //             includeAbsolute: false,
            //         },
            //     },
            // }),
        ],
        // resolve: {
        //     alias: {
        //         vue: 'vue/dist/vue.esm-bundler.js',
        //     },
        // },
    });
    ```
- Install frontend vite react plugins
    ```
        "@headlessui/react": "^2.0.0",
        "@inertiajs/react": "^1.0.0",
        "@popperjs/core": "^2.11.6",
        "@vitejs/plugin-react": "^4.3.4",
        "@vitejs/plugin-vue": "^4.0.0",
    ```
- Create resource/js/app.jsx
    ```
    import '../css/app.css';
    import './bootstrap';

    import { createInertiaApp } from '@inertiajs/react';
    import { resolvePageComponent } from 'laravel-vite-plugin/inertia-helpers';
    import { createRoot, hydrateRoot } from 'react-dom/client';

    const appName = import.meta.env.VITE_APP_NAME || 'Laravel';

    createInertiaApp({
        title: (title) => `${title} - ${appName}`,
        resolve: (name) =>
            resolvePageComponent(
                `./Pages/${name}.jsx`,
                import.meta.glob('./Pages/**/*.jsx'),
            ),
        setup({ el, App, props }) {
            if (import.meta.env.SSR) {
                hydrateRoot(el, <App {...props} />);
                return;
            }

            createRoot(el).render(<App {...props} />);
        },
        progress: {
            color: '#4B5563',
        },
    });
    ```
- Create resource/js/ssr.jsx
    ```
    import { createInertiaApp } from '@inertiajs/react';
    import createServer from '@inertiajs/react/server';
    import { resolvePageComponent } from 'laravel-vite-plugin/inertia-helpers';
    import ReactDOMServer from 'react-dom/server';
    import { route } from '../../vendor/tightenco/ziggy';

    const appName = import.meta.env.VITE_APP_NAME || 'Laravel';

    createServer((page) =>
        createInertiaApp({
            page,
            render: ReactDOMServer.renderToString,
            title: (title) => `${title} - ${appName}`,
            resolve: (name) =>
                resolvePageComponent(
                    `./Pages/${name}.jsx`,
                    import.meta.glob('./Pages/**/*.jsx'),
                ),
            setup: ({ App, props }) => {
                global.route = (name, params, absolute) =>
                    route(name, params, absolute, {
                        ...page.props.ziggy,
                        location: new URL(page.props.ziggy.location),
                    });

                return <App {...props} />;
            },
        }),
    );
    ```
- Now Controller response can be like 
```
use Inertia\Inertia;

class EventsController extends Controller
{
    public function show(Event $event)
    {
        return Inertia::render('Event/Show', [
            'event' => $event->only(
                'id',
                'title',
                'start_date',
                'description'
            ),
        ]);
    }
}
```