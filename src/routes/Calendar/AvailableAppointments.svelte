<script lang="ts">
    export let getAppointmentDate: (date: Date) => Date[]
    export let appointmentsData: Date[] = []

    // выглядит как тут не нужный, скорее как дополнительный property для calendar и компонента-композиции
    const months: string[] = ['января', 'февраля', 'марта', 'апреля', 'мая', 'июня', 'июля', 'августа', 'сентября', 'октября', 'ноября', 'декабря']

    let actualDate: Date = null;
    let actualButton: number = -1;

    function setActualDate(date: Date, i: number) {
        if (actualButton === i) {
            actualButton = -1
            actualDate = null
        } else {
            actualButton = i
            actualDate = date
        }
    }

    function handleClick(date: Date) {
        getAppointmentDate(date)
    }

</script>
{#if appointmentsData.length > 0}
    <div class="container mt-2">
        <div>Доступное время</div>
        <div class="grid grid-cols-4 gap-2 mt-2">
            {#each appointmentsData as item, i}
                <button class="btn w-16 h-6 p-0 rounded-none min-h-0 border border-solid border-gray-200 {i === actualButton ? 'btn-active btn-primary' : 'btn-ghost'}" on:click={() => setActualDate(item, i)}>{item.getHours()}:{item.getMinutes() === 0 ? `00` : item.getMinutes()}</button>
            {/each}
        </div>
        {#if !!actualDate}
            <!-- это скорее стоит поднять наверх в компонент-композицию -->
            <div class="text-xs mt-2">Назначить встречу на {actualDate.getDate()} {months[actualDate.getMonth()]} {actualDate.getFullYear()} в
                {actualDate.getHours()}:{actualDate.getMinutes() === 0 ? `00` : actualDate.getMinutes()}
            </div>
            <button class="btn btn-primary p-0 min-h-0 mt-1.5 w-24 h-6" on:click={() => handleClick(actualDate)}>Подтвердить</button>
        {/if}
    </div>
    {:else}
    <div class="container mt-2">Нет свободных мест</div>
{/if}

<style>
    .btn {
        font-size: 11px;
        line-height: 0;
    }
    .container {
        width: 280px;
    }


</style>
