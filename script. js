/* =========================================================
   FOCUS TREE TIMER
   COMPLETE JAVASCRIPT
========================================================= */


/* =========================================================
   DEFAULT DATA
========================================================= */

const defaultData = {

    coins: 0,

    streak: 0,

    trees: 0,

    deadTrees: 0,

    totalMinutes: 0,

    completedSessions: 0,

    lastCompletedDate: null,

    history: [],

    forest: [],

    unlockedTrees: ["basic"],

    selectedTree: "basic"
};


/* =========================================================
   TREE TYPES
========================================================= */

const treeTypes = {

    basic: {
        name: "Basic Tree",
        icon: "🌳",
        price: 0,
        description: "Your first focus tree."
    },

    pine: {
        name: "Pine Tree",
        icon: "🌲",
        price: 75,
        description: "A strong evergreen tree."
    },

    cherry: {
        name: "Cherry Blossom",
        icon: "🌸",
        price: 150,
        description: "A beautiful flowering tree."
    },

    autumn: {
        name: "Autumn Tree",
        icon: "🍁",
        price: 225,
        description: "A colorful seasonal tree."
    },

    palm: {
        name: "Palm Tree",
        icon: "🌴",
        price: 300,
        description: "A relaxing tropical tree."
    },

    apple: {
        name: "Apple Tree",
        icon: "🍎",
        price: 400,
        description: "A tree full of fresh fruit."
    },

    magical: {
        name: "Magic Tree",
        icon: "✨",
        price: 600,
        description: "A special reward tree."
    },

    golden: {
        name: "Golden Tree",
        icon: "🌟",
        price: 1000,
        description: "The ultimate focus tree."
    }
};


/* =========================================================
   APP DATA
========================================================= */

let appData = structuredClone(defaultData);


/* =========================================================
   TIMER VARIABLES
========================================================= */

let selectedMinutes = 25;

let totalSeconds =
    selectedMinutes * 60;

let remainingSeconds =
    totalSeconds;

let timerInterval = null;

let animationFrame = null;

let isRunning = false;

let isPaused = false;

let sessionStartTimestamp = null;

let lastResumeTimestamp = null;

let pausedMilliseconds = 0;

let lastDisplayedSecond = null;


/* =========================================================
   DOM
========================================================= */

const timerDisplay =
    document.getElementById("timerDisplay");

const timerStatus =
    document.getElementById("timerStatus");

const timerProgress =
    document.getElementById("timerProgress");

const startBtn =
    document.getElementById("startBtn");

const pauseBtn =
    document.getElementById("pauseBtn");

const stopBtn =
    document.getElementById("stopBtn");

const customHours =
    document.getElementById("customHours");

const customMinutes =
    document.getElementById("customMinutes");

const setCustomTimeBtn =
    document.getElementById("setCustomTime");

const customTimeMessage =
    document.getElementById("customTimeMessage");

const growingTree =
    document.getElementById("growingTree");

const growthStage =
    document.getElementById("growthStage");

const growthMessage =
    document.getElementById("growthMessage");

const growthProgress =
    document.getElementById("growthProgress");

const growthPercent =
    document.getElementById("growthPercent");

const fallingLeaves =
    document.getElementById("fallingLeaves");

const fallenLeaves =
    document.getElementById("fallenLeaves");


/* =========================================================
   LOCAL STORAGE
========================================================= */

function loadData() {

    const saved =
        localStorage.getItem(
            "focusTreeData"
        );

    if (!saved) {
        appData =
            structuredClone(defaultData);

        return;
    }

    try {

        const parsed =
            JSON.parse(saved);

        appData = {
            ...structuredClone(defaultData),
            ...parsed
        };

        if (!Array.isArray(appData.history)) {
            appData.history = [];
        }

        if (!Array.isArray(appData.forest)) {
            appData.forest = [];
        }

        if (!Array.isArray(appData.unlockedTrees)) {
            appData.unlockedTrees = ["basic"];
        }

    } catch {

        appData =
            structuredClone(defaultData);
    }
}


function saveData() {

    localStorage.setItem(
        "focusTreeData",
        JSON.stringify(appData)
    );
}


/* =========================================================
   FORMAT
========================================================= */

function formatTime(seconds) {

    seconds =
        Math.max(
            0,
            Math.floor(seconds)
        );

    const hours =
        Math.floor(seconds / 3600);

    const minutes =
        Math.floor(
            (seconds % 3600) / 60
        );

    const secs =
        seconds % 60;


    if (hours > 0) {

        return (
            String(hours).padStart(2, "0") +
            ":" +
            String(minutes).padStart(2, "0") +
            ":" +
            String(secs).padStart(2, "0")
        );
    }


    return (
        String(minutes).padStart(2, "0") +
        ":" +
        String(secs).padStart(2, "0")
    );
}


function formatMinutes(minutes) {

    const h =
        Math.floor(minutes / 60);

    const m =
        minutes % 60;

    if (h > 0) {

        if (m > 0) {
            return `${h}h ${m}m`;
        }

        return `${h}h`;
    }

    return `${m} min`;
}


/* =========================================================
   TIMER CIRCLE
========================================================= */

function updateTimerCircle(percent) {

    const circumference =
        603;

    const safePercent =
        Math.max(
            0,
            Math.min(
                100,
                percent
            )
        );

    const offset =
        circumference -
        (
            safePercent / 100
        ) *
        circumference;

    timerProgress.style.strokeDashoffset =
        offset;
}


/* =========================================================
   CONTINUOUS TREE GROWTH
========================================================= */

/*
    IMPORTANT:

    There are NO growth stages here.

    1% is slightly larger than 0%.
    2% is slightly larger than 1%.
    3% is slightly larger than 2%.
    ...
    99% is slightly larger than 98%.
    100% is fully grown.

    CSS variables are continuously updated
    using the exact percentage.
*/

function updateTreeGrowth(percent) {

    const p =
        Math.max(
            0,
            Math.min(
                100,
                percent
            )
        );


    /* Main growth variable */

    growingTree.style.setProperty(
        "--growth",
        p
    );


    /* Progress bar */

    growthProgress.style.width =
        `${p}%`;

    growthPercent.textContent =
        `${p.toFixed(1)}%`;


    /* =====================================================
       CONTINUOUS TEXT
    ===================================================== */

    if (p <= 0) {

        growthStage.textContent =
            "🌱 Seed";

        growthMessage.textContent =
            "Your focus journey is ready to begin.";

    } else if (p < 20) {

        growthStage.textContent =
            "🌱 Beginning";

        growthMessage.textContent =
            "Your tiny seed is beginning to wake up.";

    } else if (p < 40) {

        growthStage.textContent =
            "🌿 Growing";

        growthMessage.textContent =
            "Your young tree is slowly reaching upward.";

    } else if (p < 60) {

        growthStage.textContent =
            "🌳 Developing";

        growthMessage.textContent =
            "The trunk and branches are getting stronger.";

    } else if (p < 80) {

        growthStage.textContent =
            "🍃 Flourishing";

        growthMessage.textContent =
            "More leaves and beautiful details are appearing.";

    } else if (p < 95) {

        growthStage.textContent =
            "🌸 Blooming";

        growthMessage.textContent =
            "Your tree is getting closer to its full potential.";

    } else if (p < 100) {

        growthStage.textContent =
            "🍎 Almost Complete";

        growthMessage.textContent =
            "Just a little more focus...";

    } else {

        growthStage.textContent =
            "🌳 Fully Grown";

        growthMessage.textContent =
            "You did it! Your focus created a beautiful tree.";
    }
}


/* =========================================================
   TIMER DISPLAY
========================================================= */

function updateTimerDisplay() {

    timerDisplay.textContent =
        formatTime(
            remainingSeconds
        );
}


/* =========================================================
   REAL-TIME TIMER LOOP
========================================================= */

function startAnimationLoop() {

    if (animationFrame) {
        cancelAnimationFrame(
            animationFrame
        );
    }


    function frame() {

        if (!isRunning) {
            return;
        }


        const now =
            Date.now();


        const elapsedMilliseconds =
            (
                now -
                lastResumeTimestamp
            ) +
            pausedMilliseconds;


        const elapsedSeconds =
            elapsedMilliseconds / 1000;


        const remaining =
            Math.max(
                0,
                totalSeconds -
                elapsedSeconds
            );


        remainingSeconds =
            remaining;


        const percent =
            totalSeconds > 0
                ? (
                    (
                        totalSeconds -
                        remaining
                    ) /
                    totalSeconds
                ) *
                100
                : 0;


        /*
            TREE GROWTH IS UPDATED
            EVERY ANIMATION FRAME.
        */

        updateTreeGrowth(percent);

        updateTimerCircle(percent);


        /*
            Timer text only needs
            second-level updating.
        */

        const currentSecond =
            Math.ceil(
                remaining
            );

        if (
            currentSecond !==
            lastDisplayedSecond
        ) {

            lastDisplayedSecond =
                currentSecond;

            timerDisplay.textContent =
                formatTime(
                    currentSecond
                );
        }


        if (remaining <= 0) {

            completeSession();

            return;
        }


        animationFrame =
            requestAnimationFrame(
                frame
            );
    }


    animationFrame =
        requestAnimationFrame(
            frame
        );
}


/* =========================================================
   START
========================================================= */

function startTimer() {

    if (isRunning) {
        return;
    }


    if (
        remainingSeconds <= 0 ||
        totalSeconds <= 0
    ) {

        remainingSeconds =
            totalSeconds;

        resetTreeVisual();
    }


    isRunning = true;

    isPaused = false;

    sessionStartTimestamp =
        sessionStartTimestamp ||
        Date.now();

    lastResumeTimestamp =
        Date.now();


    timerStatus.textContent =
        "Focusing...";


    startBtn.disabled =
        true;

    pauseBtn.disabled =
        false;

    stopBtn.disabled =
        false;


    document
        .querySelectorAll(".time-btn")
        .forEach(btn => {
            btn.disabled = true;
        });


    setCustomTimeBtn.disabled =
        true;


    startAnimationLoop();
}


/* =========================================================
   PAUSE
========================================================= */

function pauseTimer() {

    if (
        !isRunning ||
        isPaused
    ) {
        return;
    }


    isPaused = true;

    isRunning = false;


    pausedMilliseconds +=
        Date.now() -
        lastResumeTimestamp;


    if (animationFrame) {

        cancelAnimationFrame(
            animationFrame
        );

        animationFrame = null;
    }


    timerStatus.textContent =
        "Paused";

    startBtn.disabled =
        false;

    pauseBtn.disabled =
        true;

    stopBtn.disabled =
        false;

    startBtn.textContent =
        "▶ Resume";
}


/* =========================================================
   RESUME
========================================================= */

function resumeTimer() {

    if (!isPaused) {
        return;
    }


    isPaused = false;

    isRunning = true;

    lastResumeTimestamp =
        Date.now();


    timerStatus.textContent =
        "Focusing...";


    startBtn.disabled =
        true;

    pauseBtn.disabled =
        false;

    stopBtn.disabled =
        false;


    startAnimationLoop();
}


/* =========================================================
   START / RESUME BUTTON
========================================================= */

startBtn.addEventListener(
    "click",
    () => {

        if (isPaused) {
            resumeTimer();
        } else {
            startTimer();
        }

    }
);


/* =========================================================
   PAUSE BUTTON
========================================================= */

pauseBtn.addEventListener(
    "click",
    pauseTimer
);


/* =========================================================
   STOP
========================================================= */

function stopTimer() {

    if (!isRunning && !isPaused) {
        return;
    }


    isRunning = false;

    isPaused = false;


    if (animationFrame) {

        cancelAnimationFrame(
            animationFrame
        );

        animationFrame = null;
    }


    if (timerInterval) {

        clearInterval(
            timerInterval
        );

        timerInterval = null;
    }


    timerStatus.textContent =
        "Tree Lost";


    startBtn.disabled =
        true;

    pauseBtn.disabled =
        true;

    stopBtn.disabled =
        true;


    document
        .querySelectorAll(".time-btn")
        .forEach(btn => {
            btn.disabled = false;
        });


    setCustomTimeBtn.disabled =
        false;


    playTreeDeathAnimation();


    appData.deadTrees++;

    addHistory(
        "lost"
    );

    saveData();

    updateDashboard();


    setTimeout(
        () => {

            document
                .getElementById(
                    "deathModal"
                )
                .classList.add("show");

        },
        2700
    );


    setTimeout(
        () => {

            resetTimer();

        },
        3400
    );
}


stopBtn.addEventListener(
    "click",
    stopTimer
);


/* =========================================================
   COMPLETE SESSION
========================================================= */

function completeSession() {

    isRunning = false;

    isPaused = false;


    if (animationFrame) {

        cancelAnimationFrame(
            animationFrame
        );

        animationFrame = null;
    }


    remainingSeconds = 0;


    updateTimerDisplay();

    updateTimerCircle(100);

    updateTreeGrowth(100);


    timerStatus.textContent =
        "Complete!";


    startBtn.disabled =
        true;

    pauseBtn.disabled =
        true;

    stopBtn.disabled =
        true;


    document
        .querySelectorAll(".time-btn")
        .forEach(btn => {
            btn.disabled = false;
        });


    setCustomTimeBtn.disabled =
        false;


    /* Coins */

    const reward =
        calculateCoinReward(
            selectedMinutes
        );


    appData.coins +=
        reward;


    appData.trees++;

    appData.completedSessions++;

    appData.totalMinutes +=
        selectedMinutes;


    updateStreak();


    /* Add completed tree */

    appData.forest.push({

        type:
            appData.selectedTree,

        minutes:
            selectedMinutes,

        date:
            new Date().toISOString()

    });


    addHistory(
        "completed",
        selectedMinutes,
        reward
    );


    saveData();

    updateDashboard();

    renderShop();

    renderForest();

    renderHistory();


    document.getElementById(
        "rewardCoins"
    ).textContent =
        reward;


    setTimeout(
        () => {

            document
                .getElementById(
                    "celebrationModal"
                )
                .classList.add("show");

        },
        300
    );


    playSuccessSound();

    sendNotification();


    setTimeout(
        () => {

            resetTimer();

        },
        900
    );
}


/* =========================================================
   COIN REWARD
========================================================= */

function calculateCoinReward(minutes) {

    return Math.max(
        10,
        Math.floor(minutes)
    );
}


/* =========================================================
   STREAK
========================================================= */

function updateStreak() {

    const today =
        new Date();

    today.setHours(
        0, 0, 0, 0
    );


    if (!appData.lastCompletedDate) {

        appData.streak = 1;

    } else {

        const last =
            new Date(
                appData.lastCompletedDate
            );

        last.setHours(
            0, 0, 0, 0
        );


        const difference =
            Math.round(
                (
                    today - last
                ) /
                (
                    1000 *
                    60 *
                    60 *
                    24
                )
            );


        if (difference === 0) {

            /* Same day */

        } else if (difference === 1) {

            appData.streak++;

        } else {

            appData.streak = 1;
        }
    }


    appData.lastCompletedDate =
        today.toISOString();
}


/* =========================================================
   HISTORY
========================================================= */

function addHistory(
    result,
    minutes = selectedMinutes,
    coins = 0
) {

    appData.history.unshift({

        result,

        minutes,

        coins,

        tree:
            appData.selectedTree,

        date:
            new Date().toISOString()

    });


    if (
        appData.history.length > 50
    ) {

        appData.history =
            appData.history.slice(
                0,
                50
            );
    }
}


function renderHistory() {

    const container =
        document.getElementById(
            "historyList"
        );


    if (
        appData.history.length === 0
    ) {

        container.innerHTML = `
            <div class="empty-forest">
                No focus sessions yet. 🌱
            </div>
        `;

        return;
    }


    container.innerHTML =
        appData.history
            .map(item => {

                const date =
                    new Date(
                        item.date
                    );


                const dateText =
                    date.toLocaleDateString(
                        undefined,
                        {
                            day: "numeric",
                            month: "short",
                            year: "numeric"
                        }
                    );


                const isComplete =
                    item.result ===
                    "completed";


                const icon =
                    isComplete
                        ? "🌳"
                        : "🍂";


                const resultText =
                    isComplete
                        ? `+${item.coins} coins`
                        : "Tree lost";


                return `

                    <div class="history-item">

                        <div class="history-left">

                            <div class="history-icon">
                                ${icon}
                            </div>

                            <div>

                                <strong>
                                    ${isComplete
                                        ? "Focus Complete"
                                        : "Focus Stopped"}
                                </strong>

                                <small>
                                    ${formatMinutes(item.minutes)}
                                    •
                                    ${dateText}
                                </small>

                            </div>

                        </div>

                        <div class="
                            history-result
                            ${isComplete
                                ? "history-complete"
                                : "history-lost"}
                        ">
                            ${resultText}
                        </div>

                    </div>
                `;
            })
            .join("");
}


/* =========================================================
   DASHBOARD
========================================================= */

function updateDashboard() {

    document.getElementById(
        "coinCount"
    ).textContent =
        appData.coins;


    document.getElementById(
        "streakCount"
    ).textContent =
        appData.streak;


    document.getElementById(
        "treesCount"
    ).textContent =
        appData.trees;


    document.getElementById(
        "minutesCount"
    ).textContent =
        appData.totalMinutes;


    document.getElementById(
        "sessionsCount"
    ).textContent =
        appData.completedSessions;


    document.getElementById(
        "deadTreesCount"
    ).textContent =
        appData.deadTrees;
}


/* =========================================================
   TREE SHOP
========================================================= */

function renderShop() {

    const shop =
        document.getElementById(
            "treeShop"
        );


    shop.innerHTML =
        Object.entries(
            treeTypes
        )
        .map(
            ([id, tree]) => {

                const unlocked =
                    appData.unlockedTrees
                        .includes(id);

                const selected =
                    appData.selectedTree === id;


                let buttonText;


                if (selected) {

                    buttonText =
                        "✓ Selected";

                } else if (unlocked) {

                    buttonText =
                        "Select";

                } else {

                    buttonText =
                        `🪙 ${tree.price}`;
                }


                return `

                    <div class="
                        tree-item
                        ${selected
                            ? "selected"
                            : ""}
                    ">

                        <div class="tree-item-icon">
                            ${tree.icon}
                        </div>

                        <h3>
                            ${tree.name}
                        </h3>

                        <p>
                            ${tree.description}
                        </p>

                        <div class="tree-price">

                            ${
                                unlocked
                                    ? "Unlocked"
                                    : `🪙 ${tree.price}`
                            }

                        </div>

                        <button
                            class="tree-action"
                            data-tree="${id}"
                        >
                            ${buttonText}
                        </button>

                    </div>
                `;
            }
        )
        .join("");


    shop
        .querySelectorAll(
            ".tree-action"
        )
        .forEach(button => {

            button.addEventListener(
                "click",
                () => {

                    handleTreeAction(
                        button.dataset.tree
                    );

                }
            );

        });
}


/* =========================================================
   TREE SHOP ACTION
========================================================= */

function handleTreeAction(id) {

    const tree =
        treeTypes[id];


    if (
        appData.unlockedTrees
            .includes(id)
    ) {

        appData.selectedTree =
            id;

        saveData();

        renderShop();

        resetTreeVisual();

        return;
    }


    if (
        appData.coins <
        tree.price
    ) {

        alert(
            `You need ${tree.price} coins to unlock this tree.`
        );

        return;
    }


    appData.coins -=
        tree.price;


    appData.unlockedTrees.push(
        id
    );

    appData.selectedTree =
        id;


    saveData();

    updateDashboard();

    renderShop();

    resetTreeVisual();
}


/* =========================================================
   FOREST
========================================================= */

function renderForest() {

    const preview =
        document.getElementById(
            "forestPreview"
        );


    if (
        appData.forest.length === 0
    ) {

        preview.innerHTML = `
            <div class="empty-forest">
                <strong>
                    Your forest is empty.
                </strong>
                <br>
                Complete your first focus session
                to plant a tree. 🌱
            </div>
        `;

        return;
    }


    const recentTrees =
        appData.forest.slice(-15);


    preview.innerHTML =
        recentTrees
            .map(tree => {

                const type =
                    treeTypes[
                        tree.type
                    ] ||
                    treeTypes.basic;


                return `
                    <div
                        class="forest-tree"
                        title="${type.name}"
                    >
                        ${type.icon}
                    </div>
                `;

            })
            .join("");
}


function renderFullForest() {

    const forest =
        document.getElementById(
            "fullForest"
        );


    if (
        appData.forest.length === 0
    ) {

        forest.innerHTML = `
            <div class="empty-forest">
                Your forest is waiting for
                its first tree. 🌱
            </div>
        `;

        return;
    }


    forest.innerHTML =
        appData.forest
            .map(tree => {

                const type =
                    treeTypes[
                        tree.type
                    ] ||
                    treeTypes.basic;


                return `
                    <div
                        class="forest-tree"
                        title="${type.name} • ${tree.minutes} min"
                    >
                        ${type.icon}
                    </div>
                `;

            })
            .join("");
}


/* =========================================================
   TREE DEATH ANIMATION
========================================================= */

function playTreeDeathAnimation() {

    fallingLeaves.innerHTML = "";

    fallenLeaves.innerHTML = "";

    fallenLeaves.classList.remove(
        "show"
    );


    growingTree.className =
        "growing-tree stage-dead";


    const leafIcons = [
        "🍃",
        "🍂",
        "🍁",
        "🌿"
    ];


    for (
        let i = 0;
        i < 20;
        i++
    ) {

        const leaf =
            document.createElement(
                "span"
            );


        leaf.className =
            "falling-leaf";


        leaf.textContent =
            leafIcons[
                Math.floor(
                    Math.random() *
                    leafIcons.length
                )
            ];


        leaf.style.left =
            (
                30 +
                Math.random() *
                40
            ) +
            "%";


        leaf.style.top =
            (
                100 +
                Math.random() *
                120
            ) +
            "px";


        leaf.style.setProperty(
            "--random-x",
            Math.random()
        );


        leaf.style.animationDelay =
            (
                Math.random() *
                1.3
            ) +
            "s";


        leaf.style.animationDuration =
            (
                2.2 +
                Math.random() *
                1.4
            ) +
            "s";


        fallingLeaves.appendChild(
            leaf
        );
    }


    setTimeout(
        () => {

            fallenLeaves.innerHTML =
                "🍂 🍃 🍁 🍂 🍃 🍂 🍁 🍃 🍂";

            fallenLeaves.classList.add(
                "show"
            );

        },
        1600
    );


    growthStage.textContent =
        "🍂 Withered";


    growthMessage.textContent =
        "Your tree slowly withered away...";


    growthProgress.style.width =
        "0%";


    growthPercent.textContent =
        "0%";
}


/* =========================================================
   RESET TREE VISUAL
========================================================= */

function resetTreeVisual() {

    fallingLeaves.innerHTML = "";

    fallenLeaves.innerHTML = "";

    fallenLeaves.classList.remove(
        "show"
    );


    growingTree.className =
        "growing-tree";


    growingTree.style.setProperty(
        "--growth",
        "0"
    );


    updateTreeGrowth(0);

    updateTimerCircle(0);

    timerDisplay.textContent =
        formatTime(
            totalSeconds
        );
}


/* =========================================================
   RESET TIMER
========================================================= */

function resetTimer() {

    if (animationFrame) {

        cancelAnimationFrame(
            animationFrame
        );

        animationFrame = null;
    }


    isRunning = false;

    isPaused = false;

    sessionStartTimestamp =
        null;

    lastResumeTimestamp =
        null;

    pausedMilliseconds = 0;

    lastDisplayedSecond =
        null;


    totalSeconds =
        selectedMinutes * 60;

    remainingSeconds =
        totalSeconds;


    timerStatus.textContent =
        "Ready";


    startBtn.disabled =
        false;

    startBtn.textContent =
        "▶ Start Focus";


    pauseBtn.disabled =
        true;

    stopBtn.disabled =
        true;


    document
        .querySelectorAll(".time-btn")
        .forEach(btn => {
            btn.disabled = false;
        });


    setCustomTimeBtn.disabled =
        false;


    resetTreeVisual();

    updateTimerDisplay();
}


/* =========================================================
   PRESET BUTTONS
========================================================= */

document
    .querySelectorAll(".time-btn")
    .forEach(button => {

        button.addEventListener(
            "click",
            () => {

                if (isRunning) {
                    return;
                }


                const minutes =
                    Number(
                        button.dataset.minutes
                    );


                selectedMinutes =
                    minutes;

                totalSeconds =
                    minutes * 60;

                remainingSeconds =
                    totalSeconds;


                document
                    .querySelectorAll(
                        ".time-btn"
                    )
                    .forEach(btn => {

                        btn.classList.remove(
                            "active"
                        );

                    });


                button.classList.add(
                    "active"
                );


                customHours.value =
                    Math.floor(
                        minutes / 60
                    );


                customMinutes.value =
                    minutes % 60;


                customTimeMessage.textContent =
                    `Preset selected: ${formatMinutes(minutes)}.`;


                customTimeMessage.style.color =
                    "var(--green)";


                timerStatus.textContent =
                    "Ready";


                resetTreeVisual();

                updateTimerDisplay();
            }
        );

    });


/* =========================================================
   CUSTOM TIME
========================================================= */

function setCustomTime() {

    if (isRunning) {

        alert(
            "Stop the current focus session before changing the time."
        );

        return;
    }


    let hours =
        parseInt(
            customHours.value
        ) || 0;


    let minutes =
        parseInt(
            customMinutes.value
        ) || 0;


    if (minutes >= 60) {

        hours +=
            Math.floor(
                minutes / 60
            );

        minutes =
            minutes % 60;
    }


    if (hours < 0) {
        hours = 0;
    }

    if (minutes < 0) {
        minutes = 0;
    }

    if (hours > 23) {
        hours = 23;
    }


    const total =
        (
            hours * 60
        ) +
        minutes;


    if (total < 1) {

        customTimeMessage.textContent =
            "Please choose at least 1 minute.";

        customTimeMessage.style.color =
            "var(--danger)";

        return;
    }


    selectedMinutes =
        total;

    totalSeconds =
        total * 60;

    remainingSeconds =
        totalSeconds;


    document
        .querySelectorAll(
            ".time-btn"
        )
        .forEach(btn =>
            btn.classList.remove(
                "active"
            )
        );


    timerStatus.textContent =
        "Ready";


    customTimeMessage.textContent =
        `Custom time set: ${formatMinutes(total)}.`;

    customTimeMessage.style.color =
        "var(--green)";


    resetTreeVisual();

    updateTimerDisplay();
}


setCustomTimeBtn.addEventListener(
    "click",
    setCustomTime
);


/* =========================================================
   CELEBRATION MODAL
========================================================= */

const celebrationModal =
    document.getElementById(
        "celebrationModal"
    );


document
    .getElementById(
        "closeCelebrationBtn"
    )
    .addEventListener(
        "click",
        () => {

            celebrationModal
                .classList.remove(
                    "show"
                );

        }
    );


/* =========================================================
   DEATH MODAL
========================================================= */

const deathModal =
    document.getElementById(
        "deathModal"
    );


document
    .getElementById(
        "closeDeathBtn"
    )
    .addEventListener(
        "click",
        () => {

            deathModal
                .classList.remove(
                    "show"
                );

        }
    );


/* =========================================================
   FOREST MODAL
========================================================= */

const forestModal =
    document.getElementById(
        "forestModal"
    );


document
    .getElementById(
        "openForestBtn"
    )
    .addEventListener(
        "click",
        () => {

            renderFullForest();

            forestModal
                .classList.add(
                    "show"
                );

        }
    );


document
    .getElementById(
        "closeForestBtn"
    )
    .addEventListener(
        "click",
        () => {

            forestModal
                .classList.remove(
                    "show"
                );

        }
    );


forestModal.addEventListener(
    "click",
    event => {

        if (
            event.target ===
            forestModal
        ) {

            forestModal
                .classList.remove(
                    "show"
                );
        }

    }
);


/* =========================================================
   SOUND
========================================================= */

let soundEnabled = true;


const soundBtn =
    document.getElementById(
        "soundBtn"
    );


soundBtn.addEventListener(
    "click",
    () => {

        soundEnabled =
            !soundEnabled;


        soundBtn.textContent =
            soundEnabled
                ? "🔊"
                : "🔇";

    }
);


function playSuccessSound() {

    if (!soundEnabled) {
        return;
    }


    try {

        const AudioContext =
            window.AudioContext ||
            window.webkitAudioContext;


        const audio =
            new AudioContext();


        const oscillator =
            audio.createOscillator();


        const gain =
            audio.createGain();


        oscillator.type =
            "sine";


        oscillator.frequency.value =
            523.25;


        gain.gain.setValueAtTime(
            0.001,
            audio.currentTime
        );


        gain.gain.exponentialRampToValueAtTime(
            0.15,
            audio.currentTime + 0.05
        );


        gain.gain.exponentialRampToValueAtTime(
            0.001,
            audio.currentTime + 0.8
        );


        oscillator.connect(
            gain
        );

        gain.connect(
            audio.destination
        );


        oscillator.start();

        oscillator.stop(
            audio.currentTime + 0.8
        );

    } catch {

        /* Audio not available */
    }
}


/* =========================================================
   DARK MODE
========================================================= */

const darkModeBtn =
    document.getElementById(
        "darkModeBtn"
    );


const savedTheme =
    localStorage.getItem(
        "focusTreeTheme"
    );


if (savedTheme === "dark") {

    document.body.classList.add(
        "dark"
    );

    darkModeBtn.textContent =
        "☀️";
}


darkModeBtn.addEventListener(
    "click",
    () => {

        document.body.classList.toggle(
            "dark"
        );


        const dark =
            document.body.classList.contains(
                "dark"
            );


        darkModeBtn.textContent =
            dark
                ? "☀️"
                : "🌙";


        localStorage.setItem(
            "focusTreeTheme",
            dark
                ? "dark"
                : "light"
        );

    }
);


/* =========================================================
   NOTIFICATIONS
========================================================= */

async function requestNotificationPermission() {

    if (
        "Notification" in window &&
        Notification.permission ===
        "default"
    ) {

        try {

            await Notification.requestPermission();

        } catch {

            /* Ignore */
        }
    }
}


function sendNotification() {

    if (
        "Notification" in window &&
        Notification.permission ===
        "granted"
    ) {

        new Notification(
            "Focus Tree 🌳",
            {
                body:
                    "Your focus session is complete! Your tree has fully grown."
            }
        );
    }
}


/* =========================================================
   VISIBILITY
========================================================= */

document.addEventListener(
    "visibilitychange",
    () => {

        /*
            The timer is based on Date.now(),
            so leaving the tab does not make
            the timer incorrectly freeze.
        */

        if (
            document.visibilityState ===
            "visible" &&
            isRunning
        ) {

            startAnimationLoop();
        }

    }
);


/* =========================================================
   INITIALIZE
========================================================= */

function initializeApp() {

    loadData();

    updateDashboard();

    renderShop();

    renderForest();

    renderHistory();

    resetTimer();

    requestNotificationPermission();

}


initializeApp();
