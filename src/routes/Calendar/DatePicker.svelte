<script lang="ts">
    import {beforeUpdate} from 'svelte';
    export let getCheckedDate: (date: Date|null) => void;
    export let points: {quantity: number, date: Date}[] = []
    export let title: string = 'Назначить встречу'

    const monthNames = ["January", "February", "March", "April", "May", "June",
        "July", "August", "September", "October", "November", "December"
    ];
    const daysInWeek = ['Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su']

    const date: {year: number, month: number} = {year: new Date().getFullYear(), month: new Date().getMonth()}
    let days: number = daysInMonth(date.year, date.month)
    let firstWeekDay: number = firstMonthDay(date.year, date.month)
    let arr: number[] = []
    let actualButton: number = -1

    function daysInMonth(year: number, month: number): number {
        return 33 - new Date(year, month, 33).getDate()
    }

    function firstMonthDay(year: number, month: number): number {
        return new Date(year, month, 1).getDay()
    }

    function fillArray(): number[] {
        const arr: number[] = []
        if (firstWeekDay !== 1) {
            const daysInPreviousMonth = daysInMonth(date.year, date.month - 1)
            for (let i = firstWeekDay - 1; i > 0; i--) {
                arr.push(daysInPreviousMonth - (i - 1))
            }
        }
        for (let i = 1; i <= days; i++) {
            arr.push(i)
        }
        let i = 1
        do {
            arr.push(i)
            i++
        } while (arr.length < 42)
        return arr;
    }

    function setActiveButton(i: number ,day: number) {
        if (i === actualButton) {
            getCheckedDate(null)
            actualButton = -1
        } else {
            getCheckedDate(new Date(date.year, date.month, day))
            actualButton = i
        }
    }

    function pointsInDay(day: number) {
        if (points.length > 0) {
            for (let j = 0; j < points.length; j++) {
                if (points[j].date.getFullYear() === date.year && points[j].date.getMonth() === date.month && points[j].date.getDate() === day) {
                    return points[j].quantity
                }
            }
        }
        return 0
    }

    function decreaseMonth() {
        if (date.month !== 0) {
            date.month -= 1
        } else {
            date.year -= 1
            date.month = 11
        }
        actualButton = -1
        getCheckedDate(null)
    }

    function increaseMonth() {
        if (date.month !== 11) {
            date.month += 1
        } else {
            date.year += 1
            date.month = 0
        }
        actualButton = -1
        getCheckedDate(null)
    }

    beforeUpdate(() => {
        days = daysInMonth(date.year, date.month)
        firstWeekDay = firstMonthDay(date.year, date.month)
        arr = fillArray()
    })

</script>
<div class="container">
    <div class="font-bold mb-3 text-lg leading-6">{title}</div>
    <div class="flex justify-between">
        <div class="font-bold text-lg leading-6 underline">{monthNames[date.month]} {date.year}</div>
        <div class="flex">
            <button class="btn btn-sm btn-ghost" on:click={decreaseMonth}>
                <svg width="9" height="15" viewBox="0 0 9 15" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M7.54289 0.542908L8.95711 1.95712L3.16421 7.75001L8.95711 13.5429L7.54289 14.9571L0.335787 7.75001L7.54289 0.542908Z" fill="black"/>
                </svg>
            </button>
            <button class="btn btn-sm btn-ghost" on:click={increaseMonth}>
                <svg class="rotate-180" width="9" height="15" viewBox="0 0 9 15" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M7.54289 0.542908L8.95711 1.95712L3.16421 7.75001L8.95711 13.5429L7.54289 14.9571L0.335787 7.75001L7.54289 0.542908Z" fill="black"/>
                </svg>
            </button>
        </div>
    </div>
    <div class="grid grid-cols-7 h-10">
        {#each daysInWeek as day}
            <div class="text-xs font-semibold flex justify-center items-center">{day}</div>
        {/each}
    </div>
    <table>
        {#each arr as item, index}
            {#if index === 0 || index % 7 === 0}
                <tr>
                    {#each arr as item, i}
                        {#if i >= index && i <= index + 6}
                            {#if firstWeekDay - 1 > i || i > days + firstWeekDay - 2}
                                <td class="text-xs w-10 h-10 text-center p-0 relative border border-solid border-gray-200">
                                    <button class="btn rounded-none w-full h-full p-0 min-h-0" disabled="disabled">{item}</button>
                                </td>
                            {:else}
                                <td class="text-xs w-10 h-10 text-center p-0 relative border border-solid border-gray-200 cursor-pointer"  on:click={() => setActiveButton(i, item)}>
                                    <button class="btn rounded-none btn-xs w-full h-full {actualButton === i ? 'btn-primary btn-active' : 'btn-ghost'}">{item}</button>
                                    {#if pointsInDay(item) === 1}
                                        <div class="flex absolute bottom-1 w-full justify-center {actualButton === i ? 'z-10' : 'z-index'}">
                                            <div class="circle rounded-full circle_blue"></div>
                                        </div>
                                    {:else if pointsInDay(item) === 2}
                                        <div class="flex absolute bottom-1 w-full justify-center {actualButton === i ? 'z-10' : 'z-index'}">
                                            <div class="circle rounded-full circle_blue"></div>
                                            <div class="circle rounded-full circle_green"></div>
                                        </div>
                                    {:else if pointsInDay(item) === 3}
                                        <div class="flex absolute bottom-1 w-full justify-center {actualButton === i ? 'z-10' : 'z-index'}">
                                            <div class="circle rounded-full circle_blue"></div>
                                            <div class="circle rounded-full circle_green"></div>
                                            <div class="circle rounded-full circle_grey"></div>
                                        </div>
                                    {:else if pointsInDay(item) === 4}
                                        <div class="flex absolute bottom-1 w-full justify-center {actualButton === i ? 'z-10' : 'z-index'}">
                                            <div class="circle rounded-full circle_blue -ml-0.5"></div>
                                            <div class="circle rounded-full circle_green -ml-0.5"></div>
                                            <div class="circle rounded-full circle_grey -ml-0.5"></div>
                                            <div class="circle rounded-full circle_brown -ml-0.5"></div>
                                        </div>
                                    {:else if pointsInDay(item) >= 5}
                                        <div class="flex absolute bottom-1 w-full justify-center {actualButton === i ? 'z-10' : 'z-index'}">
                                            <div class="circle rounded-full circle_blue -ml-0.5"></div>
                                            <div class="circle rounded-full circle_green -ml-0.5"></div>
                                            <div class="circle rounded-full circle_grey -ml-0.5"></div>
                                            <div class="circle rounded-full circle_brown -ml-0.5"></div>
                                            <div class="circle rounded-full circle_yellow -ml-0.5"></div>
                                        </div>
                                    {/if}
                                </td>
                            {/if}
                        {/if}
                    {/each}
                </tr>
            {/if}
        {/each}
    </table>
</div>


<style>
    .z-index {
        z-index: -1;
    }
    .container {
        width: 280px;
    }

    .circle {
        width: 6px;
        height: 5px;
    }

    .circle_blue {
        background-color: cornflowerblue;
    }

    .circle_green {
        background-color: #0E6100;
    }

    .circle_grey {
        background-color: #C4C4C4;
    }

    .circle_brown {
        background-color: #730404;
    }

    .circle_yellow {
        background-color: yellow;
    }
</style>