const os = require("os");
const moment = require("moment-timezone");
const env = require("../../settings");
const {
    getTopCommands
} = require("../../core/stats");

const handler = async (m, {
    conn,
    user,
    store,
    isOwner,
    isPremium
}) => {
    const formatTime = (s) => {
        const h = Math.floor(s / 3600);
        const m = Math.floor((s % 3600) / 60);
        const sec = Math.floor(s % 60);
        return [h, m, sec].map(v => String(v).padStart(2, '0')).join(':');
    };

    const topRaw = getTopCommands(3);

    const top = getTopCommands(3); // array of objects: [{ command: 'menu', count: 50 }, ...]
    const groupCount = Object.keys(await conn.groupFetchAllParticipating()).length;
    const topText = top.length ?
        top.map((v, i) => `🔹 *${v.command}* (${v.count}x)`).join('\n')

        :
        'Belum ada data~';

    const userStatus = isOwner ?
        "👑 Owner" :
        isPremium ?
        "💎 Premium" :
        user.registered ?
        "👤 Free User" :
        "❓ Not Registered";

    const teks = `Hai Kak *${m.name || "User"}*! Aku *${env.nameBot}*, siap nemenin harimu 🌸

• 👤 Status: ${userStatus}
• 💎 Limit: ${user.limit || 0}
• ⏱️ Uptime: ${formatTime(process.uptime())}
• 💻 Server: ${formatTime(os.uptime())}
• 👥 Grup: ${groupCount}
• 📊 Top Commands\n${topText}

Jangan lupa makan dan senyum hari ini~ 🫶`;
    conn.sendMessage(m.chat, {
        footer: env.footer,
        buttons: [{
                buttonId: 'action',
                buttonText: {
                    displayText: 'ini pesan interactiveMeta'
                },
                type: 4,
                nativeFlowInfo: {
                    name: 'single_select',
                    paramsJson: JSON.stringify({
                        title: 'Click To List',
                        sections: [{
                            title: 'LIST MENU',
                            highlight_label: 'Recomend',
                            rows: [{
                                    title: 'AllMenu ⚡',
                                    description: 'Menampilkan Allmenu',
                                    id: '.allmenu'
                                },
                                {
                                    title: 'MenuStore 🛍',
                                    description: 'Menampilkan Store Menu',
                                    id: '.menustore'
                                },
                                {
                                    title: 'RekapMenu 🖨',
                                    description: 'Menampilkan Rekap Menu',
                                    id: '.menurekap'
                                },
                                {
                                    title: 'MainMenu 🚂',
                                    description: 'Menampilkan Main Menu',
                                    id: '.menumain'
                                },
                                {
                                    title: 'MakerMenu 🗺',
                                    description: 'Menampilkan Maker Menu',
                                    id: '.menumaker'
                                },
                                {
                                    title: 'TolsMenu 🔗',
                                    description: 'Menampilkan Tols Menu',
                                    id: '.menutools'
                                },
                                {
                                    title: 'SearchMenu 📡',
                                    description: 'Menampilkan Search Menu',
                                    id: '.menusearch'
                                },
                                {
                                    title: 'DownloadMenu 📥',
                                    description: 'Menampilkan Download Menu',
                                    id: '.menudownloader'
                                },
                                {
                                    title: 'GroupMenu 🗝',
                                    description: 'Menampilkan Group Menu',
                                    id: '.menugroup'
                                },
                                {
                                    title: 'PanelMenu 🕹',
                                    description: 'Menampilkan Panel Menu',
                                    id: '.menupanel'
                                },
                                {
                                    title: 'ConvertMenu 🚀',
                                    description: 'Menampilkan Convert Menu',
                                    id: '.menuconvert'
                                },
                                {
                                    title: 'FunMenu 😂',
                                    description: 'Menampilkan Fun Menu',
                                    id: '.menufun'
                                },
                                {
                                    title: 'RPGMenu ⚔️',
                                    description: 'Menampilkan RPG Menu',
                                    id: '.menurpg'
                                },
                                {
                                    title: 'GameMenu 🎮',
                                    description: 'Menampilkan Game Menu',
                                    id: '.menugame'
                                }
                            ]
                        }]
                    })
                }
            },
            {
                buttonId: `.owner`,
                buttonText: {
                    displayText: 'Owner👑'
                },
                type: 1
            },
            {
                buttonId: `.ping`,
                buttonText: {
                    displayText: 'Speed⚡'
                },
                type: 1
            }



        ],
        image: {
            url: env.thumb
        },
        caption: teks,
        headerType: 1,
        viewOnce: true
    }, {
        quoted: m
    });
};

handler.command = ['menu'];
handler.category = 'info';
handler.description = 'Menampilkan menu ringkas dengan top command';
handler.register = true;

module.exports = handler;