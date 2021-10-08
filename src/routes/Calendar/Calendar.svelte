<script lang="ts">
    import DatePicker from './DatePicker.svelte'
    import AvailableAppointments from './AvailableAppointments.svelte'

    // chosen date?
    let actualDate: Date|null = null;

    function getDate(date: Date|null) {
        actualDate = date
    }

    function getAppointmentsData(actualDate: Date): Date[] {
        if (actualDate) {
            return arrForPoints.reduce((res: Date[], item) => {
                if (actualDate.getFullYear() === item.getFullYear() && actualDate.getMonth() === item.getMonth() && actualDate.getDate() === item.getDate()) {
                    res.push(item)
                }
                return res
            }, [])
        }
        return []
    }
    // это должен быть property у композиции и у datePicker тоже
    const arrForPoints: Date[] = [
        new Date(2021, 8, 15, 16, 35),
        new Date(2021, 10, 8, 18, 40),
        new Date(2021, 9, 5, 12, 30),
        new Date(2021, 9, 5, 13, 0),
        new Date(2021, 9, 5, 13, 30),
        new Date(2021, 9, 5, 15, 0),
        new Date(2021, 9, 5, 20, 0),
        new Date(2021, 9, 6, 14, 15),
        new Date(2021, 9, 6, 17, 25),
        new Date(2021, 9, 6, 18, 40),
    ]
    // тож проперти participant_id цвета раздаются по юзерам
    const arrForAppointments: {quantity: number, date: Date}[] = [
        {quantity: 0, date: new Date(2021, 9, 1)},
        {quantity: 1, date: new Date(2021, 9, 3)},
        {quantity: 2, date: new Date(2021, 9, 5)},
        {quantity: 3, date: new Date(2021, 9, 7)},
        {quantity: 4, date: new Date(2021, 9, 9)},
        {quantity: 5, date: new Date(2021, 9, 11)},
        {quantity: 6, date: new Date(2021, 9, 13)},
        {quantity: 7, date: new Date(2021, 9, 15)},
    ]

    function getAppointmentDate(date: Date) {
        console.log(date)
    }
</script>

<!-- Сам по себе компонент Calendar у тебя скорее AppointmentScheduler
    так как он ставит в композицию календарь(date picker) и выбор доступного времени
-->

<!-- не самый лучший паттерн прокидывать функцию манипулирующую родительским состоянием
для таких как раз случае есть событийная модель, типо onDateSelected -->
<DatePicker getCheckedDate={getDate} points={arrForAppointments}/>

{#if actualDate}
    <AvailableAppointments appointmentsData={getAppointmentsData(actualDate)} getAppointmentDate={getAppointmentDate}/>
{/if}
