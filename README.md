# insulin-calculator
<!DOCTYPE html>
<html lang="en">
<head>

<meta charset="UTF-8">

<meta name="viewport"
      content="width=device-width,
               initial-scale=1,
               viewport-fit=cover">

<meta name="apple-mobile-web-app-capable"
      content="yes">

<meta name="apple-mobile-web-app-status-bar-style"
      content="default">

<meta name="apple-mobile-web-app-title"
      content="Insulin Calc">

<title>Insulin Calculator</title>

<style>

* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #f2f4f7;
    color: #101828;
    font-family:
        -apple-system,
        BlinkMacSystemFont,
        "SF Pro Display",
        "Segoe UI",
        sans-serif;
}

.app {
    max-width: 560px;
    margin: auto;

    padding:
        env(safe-area-inset-top)
        16px
        calc(30px + env(safe-area-inset-bottom));
}

header {
    padding: 22px 4px 14px;
}

h1 {
    margin: 0;
    font-size: 30px;
    letter-spacing: -0.6px;
}

header p {
    margin: 5px 0 0;
    color: #667085;
}

.card {
    background: white;
    border-radius: 20px;
    padding: 18px;
    margin: 10px 0;

    box-shadow:
        0 2px 10px rgba(16,24,40,0.06);
}

label {
    display: block;
    font-weight: 700;
    margin: 14px 0 7px;
}

input,
select {
    width: 100%;
    height: 54px;

    border: 1px solid #d0d5dd;
    border-radius: 14px;

    padding: 0 14px;

    font-size: 18px;

    background: white;
    color: #101828;
}

button {
    width: 100%;
    height: 56px;

    border: none;
    border-radius: 15px;

    font-size: 18px;
    font-weight: 800;

    background: #111827;
    color: white;

    margin-top: 18px;
}

button:active {
    transform: scale(0.98);
}

.result {
    display: none;

    text-align: center;

    padding: 20px;

    border-radius: 18px;

    background: #f2f4f7;

    margin-top: 14px;
}

.result-title {
    color: #667085;
    font-size: 14px;
    font-weight: 700;
}

.dose {
    font-size: 48px;
    font-weight: 900;
    line-height: 1.05;

    margin: 7px 0;
}

.breakdown {
    margin-top: 14px;
    text-align: left;
}

.line {
    display: flex;
    justify-content: space-between;

    padding: 11px 0;

    border-bottom:
        1px solid #eaecf0;
}

.line:last-child {
    border-bottom: none;
}

.warning {
    margin-top: 13px;

    padding: 13px;

    border-radius: 13px;

    background: #fff4e5;
    color: #7a4b00;

    font-size: 14px;
    line-height: 1.4;
}

.history-title {
    font-size: 21px;
    font-weight: 800;
    margin-bottom: 10px;
}

.empty {
    color: #667085;
    font-size: 14px;
    padding: 10px 0;
}

.log {
    border: 1px solid #eaecf0;
    border-radius: 15px;

    padding: 13px;

    margin-top: 10px;
}

.log-top {
    display: flex;

    justify-content: space-between;

    gap: 10px;
}

.muted {
    color: #667085;
    font-size: 13px;
}

.delete-button {
    width: auto;

    height: 38px;

    padding: 0 12px;

    font-size: 13px;

    margin-top: 9px;

    background: white;

    color: #b42318;

    border:
        1px solid #fecdca;
}

.secondary-button {
    background: white;

    color: #101828;

    border:
        1px solid #d0d5dd;

    height: 48px;

    font-size: 14px;

    margin-top: 10px;
}

.settings {
    font-size: 14px;

    color: #475467;

    line-height: 1.6;
}

details summary {
    font-weight: 750;

    cursor: pointer;
}

</style>

</head>

<body>

<div class="app">

<header>

<h1>
Insulin Calculator
</h1>

<p>
iPhone-friendly calculator &amp; log
</p>

</header>


<!-- CALCULATOR -->

<div class="card">

<label for="glucose">
Blood glucose (mmol/L)
</label>

<input
    id="glucose"
    type="number"
    inputmode="decimal"
    step="0.1"
    min="0"
    placeholder="e.g. 8.4"
>


<label for="carbs">
Carbohydrates (g)
</label>

<input
    id="carbs"
    type="number"
    inputmode="decimal"
    step="1"
    min="0"
    placeholder="e.g. 60"
>


<label for="meal">
Meal
</label>

<select id="meal">

<option>
Breakfast
</option>

<option>
Lunch
</option>

<option>
Dinner
</option>

<option>
Snack
</option>

</select>


<button id="calculate">
CALCULATE
</button>


<!-- RESULT -->

<div id="result"
     class="result">

</div>

</div>


<!-- HISTORY -->

<div class="card">

<div class="history-title">
Log History
</div>

<div id="history">

</div>


<button
    id="export"
    class="secondary-button">

Export Logs as CSV

</button>


<button
    id="clear"
    class="secondary-button">

Clear All Logs

</button>

</div>


<!-- SETTINGS -->

<div class="card settings">

<details>

<summary>
Your calculation settings
</summary>


<p>

Breakfast, lunch and dinner:

<b>
1 unit : 12g carbs
</b>

</p>


<p>

Blood glucose:

<br>

4.6–9.7 mmol/L:
<b>
0 units correction
</b>

<br>

9.8–13.0 mmol/L:
<b>
1 unit correction
</b>

<br>

13.1–16.0 mmol/L:
<b>
1.5 units correction
</b>

<br>

Above 16.0 mmol/L:
<b>
2 units correction
</b>

</p>

</details>

</div>


<!-- SAFETY -->

<div class="card settings">

<b>
Important
</b>

<br><br>

This calculator applies the settings entered
into it and does not determine what your medical
settings should be.

Follow your diabetes team's current instructions,
including any insulin-on-board, meal timing,
hypo and sick-day rules.

Do not rely on this calculator alone for urgent
or unexpectedly high or low readings.

</div>


</div>


<script>

"use strict";


/*
====================================================
STORAGE
====================================================
*/

const STORAGE_KEY =
    "insulinCalculatorLogs_v3";


/*
====================================================
HELPER FUNCTIONS
====================================================
*/

function getElement(id) {

    return document.getElementById(id);

}


function formatNumber(number) {

    if (Number.isInteger(number)) {

        return String(number);

    }

    return Number(number).toFixed(1);

}


function getLogs() {

    try {

        return JSON.parse(
            localStorage.getItem(STORAGE_KEY)
            || "[]"
        );

    }

    catch (error) {

        return [];

    }

}


function saveLogs(logs) {

    localStorage.setItem(

        STORAGE_KEY,

        JSON.stringify(
            logs.slice(0, 500)
        )

    );

}


/*
====================================================
CALCULATE CORRECTION
====================================================
*/

function getCorrection(glucose) {

    /*
    Below 4.6 is handled separately.
    */

    if (glucose <= 9.7) {

        return 0;

    }


    if (glucose <= 13.0) {

        return 1;

    }


    if (glucose <= 16.0) {

        return 1.5;

    }


    return 2;

}


/*
====================================================
DISPLAY HISTORY
====================================================
*/

function renderHistory() {

    const history =
        getElement("history");

    const logs =
        getLogs();


    if (logs.length === 0) {

        history.innerHTML =
            '<div class="empty">' +
            'No calculations logged yet.' +
            '</div>';

        return;

    }


    history.innerHTML =
        logs.map(function(log) {

            const date =
                new Date(log.time)
                .toLocaleString(
                    [],
                    {
                        dateStyle: "medium",
                        timeStyle: "short"
                    }
                );


            return `

                <div class="log">

                    <div class="log-top">

                        <b>
                            ${log.meal}
                        </b>

                        <span class="muted">
                            ${date}
                        </span>

                    </div>


                    <div style="margin-top:8px">

                        Blood glucose:

                        <b>
                            ${log.glucose}
                            mmol/L
                        </b>

                    </div>


                    <div style="margin-top:5px">

                        Carbohydrates:

                        <b>
                            ${log.carbs}g
                        </b>

                    </div>


                    <div style="margin-top:5px">

                        Carb dose:

                        <b>
                            ${formatNumber(log.carbDose)}u
                        </b>

                    </div>


                    <div style="margin-top:5px">

                        Correction:

                        <b>
                            ${formatNumber(log.correction)}u
                        </b>

                    </div>


                    <div style="margin-top:5px">

                        Calculated total:

                        <b>
                            ${formatNumber(log.total)}u
                        </b>

                    </div>


                    <button
                        class="delete-button"
                        data-id="${log.id}">

                        Delete

                    </button>

                </div>

            `;

        }).join("");


    document
        .querySelectorAll(".delete-button")
        .forEach(function(button) {

            button.addEventListener(
                "click",
                function() {

                    const id =
                        this.dataset.id;


                    const remaining =
                        getLogs().filter(
                            function(log) {

                                return String(log.id)
                                    !== String(id);

                            }
                        );


                    saveLogs(remaining);

                    renderHistory();

                }
            );

        });

}


/*
====================================================
CALCULATE BUTTON
====================================================
*/

getElement("calculate")
.addEventListener(
    "click",
    function() {


        const glucose =
            Number(
                getElement("glucose").value
            );


        const carbs =
            Number(
                getElement("carbs").value
            );


        const meal =
            getElement("meal").value;


        const result =
            getElement("result");


        /*
        Validate inputs.
        */

        if (

            !Number.isFinite(glucose) ||

            !Number.isFinite(carbs) ||

            glucose < 0 ||

            carbs < 0

        ) {

            result.style.display =
                "block";


            result.innerHTML = `

                <div class="warning">

                    <b>
                    Please enter both your
                    blood glucose and carbs.
                    </b>

                </div>

            `;

            return;

        }


        /*
        Low glucose safeguard.
        */

        if (glucose < 4.6) {

            result.style.display =
                "block";


            result.innerHTML = `

                <div class="warning">

                    <b>
                    Low glucose
                    </b>

                    <br><br>

                    No insulin dose is calculated
                    below 4.6 mmol/L.

                    Follow your hypo treatment
                    plan and recheck as instructed
                    by your diabetes team.

                </div>

            `;

            return;

        }


        /*
        Carb calculation.

        Your ratio:

        1 unit per 12g carbohydrate.
        */

        const carbDose =
            carbs / 12;


        /*
        Correction calculation.
        */

        const correction =
            getCorrection(glucose);


        /*
        Total calculated amount.
        */

        const total =
            carbDose + correction;


        /*
        Save the calculation.
        */

        const newLog = {

            id:
                Date.now(),

            time:
                new Date().toISOString(),

            glucose:
                glucose,

            carbs:
                carbs,

            meal:
                meal,

            carbDose:
                carbDose,

            correction:
                correction,

            total:
                total

        };


        const logs =
            getLogs();


        logs.unshift(newLog);


        saveLogs(logs);


        /*
        Display result.
        */

        result.style.display =
            "block";


        result.innerHTML = `

            <div class="result-title">

                CALCULATED INSULIN DOSE

            </div>


            <div class="dose">

                ${formatNumber(total)}
                units

            </div>


            <div class="breakdown">


                <div class="line">

                    <span>
                        Carbohydrate dose
                    </span>

                    <b>
                        ${formatNumber(carbDose)}
                        units
                    </b>

                </div>


                <div class="line">

                    <span>
                        Glucose correction
                    </span>

                    <b>
                        ${formatNumber(correction)}
                        units
                    </b>

                </div>


            </div>


            ${
                glucose > 16

                ?

                `

                <div class="warning">

                    <b>
                    High reading
                    </b>

                    <br>

                    Follow your diabetes team's
                    ketone/sick-day guidance if
                    applicable.

                </div>

                `

                :

                ""

            }

        `;


        /*
        Update history.
        */

        renderHistory();

    }

);


/*
====================================================
CLEAR ALL LOGS
====================================================
*/

getElement("clear")
.addEventListener(
    "click",
    function() {

        const logs =
            getLogs();


        if (logs.length === 0) {

            return;

        }


        const confirmed =
            confirm(
                "Delete all saved logs from this device?"
            );


        if (confirmed) {

            localStorage.removeItem(
                STORAGE_KEY
            );

            renderHistory();

        }

    }
);


/*
====================================================
EXPORT CSV
====================================================
*/

getElement("export")
.addEventListener(
    "click",
    function() {


        const logs =
            getLogs();


        if (logs.length === 0) {

            alert(
                "There are no logs to export."
            );

            return;

        }


        const rows = [

            [

                "Date/time",

                "Meal",

                "Blood glucose (mmol/L)",

                "Carbs (g)",

                "Carb dose (units)",

                "Correction (units)",

                "Calculated total (units)"

            ]

        ];


        logs.forEach(
            function(log) {

                rows.push([

                    new Date(log.time)
                    .toLocaleString(),

                    log.meal,

                    log.glucose,

                    log.carbs,

                    formatNumber(
                        log.carbDose
                    ),

                    formatNumber(
                        log.correction
                    ),

                    formatNumber(
                        log.total
                    )

                ]);

            }
        );


        const csv =
            rows
            .map(function(row) {

                return row
                .map(function(value) {

                    return '"' +
                        String(value)
                        .replace(
                            /"/g,
                            '""'
                        ) +
                        '"';

                })
                .join(",");

            })
            .join("\n");


        const blob =
            new Blob(
                [csv],
                {
                    type:
                        "text/csv;charset=utf-8;"
                }
            );


        const url =
            URL.createObjectURL(blob);


        const link =
            document.createElement("a");


        link.href =
            url;


        link.download =
            "insulin-logs.csv";


        document.body.appendChild(link);


        link.click();


        document.body.removeChild(link);


        URL.revokeObjectURL(url);

    }
);


/*
====================================================
START APP
====================================================
*/

renderHistory();

</script>

</body>
</html>