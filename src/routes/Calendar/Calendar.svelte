<script lang="ts">
    import DatePicker from './DatePicker.svelte'
    import AvailableAppointments from './AvailableAppointments.svelte'

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

<DatePicker getCheckedDate={getDate} points={arrForAppointments}/>

{#if actualDate}
    <AvailableAppointments appointmentsData={getAppointmentsData(actualDate)} getAppointmentDate={getAppointmentDate}/>
{/if}