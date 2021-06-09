# 🚌校园巴士 - 自动跳转

## 根据国家法定节假日和周末自动跳转。如停留在此页面，请刷新。

## 🚌校园巴士

<div id="button-div">
<div class='bt-sub'><a href="./workday.html">💼 工作日</a></div>
<div class='bt-sub'><a href="./holiday.html">💤 节假日</a></div>
</div>

<ClientOnly>
<style>
.bt-sub {
    margin-top: 1%;
    display: inline-block;
    width: 48%;
    text-align: center;
}
</style>
</ClientOnly>


<script>
  export default {
    mounted () {
    function bus_redirect(){
        var day_map = {};
        // JSON is from https://github.com/NateScarlet/holiday-cn
        // need to update by year
        $.getJSON("/2021.json", function (data) {
            for (let i = 0; i < data.days.length; i++) {
                item = data.days[i];
                day_map[item.date] = item.isOffDay;
            }
        });
        // console.log(day_map);
        var now_date = new Date();
        var ye = new Intl.DateTimeFormat('en', {year: 'numeric'}).format(now_date);
        var mo = new Intl.DateTimeFormat('en', {month: '2-digit'}).format(now_date);
        var da = new Intl.DateTimeFormat('en', {day: '2-digit'}).format(now_date);
        var day_key = `${ye}-${mo}-${da}`;
        var day_in_week = now_date.getDay();
        var is_holiday;
        if (day_map[day_key] == null) {
            var isWeekend = (day_in_week == 6) || (day_in_week == 0);
            // 6 = Saturday, 0 = Sunday
            is_holiday = isWeekend;
            console.log(isWeekend);
            console.log("No in GOV declaration")
        } else {
            is_holiday = day_map[day_key];
            console.log(day_map[day_key]);
            console.log("In GOV declaration")
        }
        if (is_holiday){
            location.href = "/transport/holiday.html";
        }else {
            location.href = "/transport/workday.html";
        }
    }

    document.addEventListener('DOMContentLoaded', bus_redirect, false);

    $(document).ready(function () {
        bus_redirect();
    });
    setInterval(bus_redirect, 1000);
    }
  }
</script>
