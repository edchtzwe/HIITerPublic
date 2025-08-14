<script>
    import { timerStore } from '../stores/timer.js';
    import { onMount, onDestroy } from 'svelte';

    let interval;

    onMount(() => {
        timerStore.update(s => {
            return { ...s, backgroundColor: "#3b82f6" };
        });

        interval = setInterval(() => {
            timerStore.update(s => {
                if (s.currentPhase !== 'countdown' || !s.isRunning) {
                    clearInterval(interval);
                    interval = null;
                    return s;
                }

                if (s.timeRemaining > 0) {
                    return { ...s, timeRemaining: s.timeRemaining - 1 };
                }

                clearInterval(interval);
                interval = null;
                return {
                    ...s,
                    currentPhase: 'active',
                    timeRemaining: s.active
                };
            });
        }, 1000);
    });

    onDestroy(() => {
        clearInterval(interval);
    });
</script>

<div class="text-center">
    <h1 class="text-[20rem] font-black text-white leading-none">
        {$timerStore.timeRemaining}
    </h1>
    <p class="text-8xl font-black text-white/90 mt-8">GET READY!</p>
</div>
