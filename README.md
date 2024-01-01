# WhatsApp Web API - TypeScript/JavaScript
WhatsApp Web API ini ditenagai oleh library: Baileys

## Instalasi
### Menggunakan Npm
```bash
npm i whatsapp-web-api
```

### Atau Menggunakan Yarn
```bash
yarn add whatsapp-web-api
```

## Contoh Kode
### ./package.json
```json
{
  "name": "whatsapp-bot",
  "version": "0.0.0",
  "private": true,
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "whatsapp-web-api": "latest"
  },
  "devDependencies": {
    "nodemon": "~3.0.1"
  }
}
```

### ./nodemon.json
```json
{
  "ignore": ["index.js"],
  "ext": "js"
}
```

### ./index.js
```javascript
const wwa = require('whatsapp-web-api');
const fs = require('fs');
const chalk = require('chalk');
const config = require('./config/settings.js');
const client = require('./includes/client.js');

const {
  makeWASocket
} = wwa;

const startServer = (config, client) => {
  return makeWASocket(config, client);
};

startServer(config, client);
```

### ./config/settings.js
```javascript
module.exports = {
  pairing_mode: true,
  session_folder_name: 'session',
  browser: ["Chrome (Linux)"],
  public_mode: true,
  prefix: '.'
};
```

### ./includes/client.js
```javascript
const chalk = require('chalk');
const config = require('../config/settings.js');

module.exports = async ({
  client,
  msg,
  store
}) => {
  try {
    const body = msg.mtype === 'conversation' ? msg.message.conversation : msg.mtype === 'extendedTextMessage' ? msg.message.extendedTextMessage.text : '';
    const budy = typeof msg.text === 'string' ? msg.text : '';
    const isCommand = body.startsWith(config.prefix) ? body.replace(config.prefix, '').trim().split(/ +/).shift().toLowerCase() : '';
    const command = isCommand.replace(config.prefix, '');
    const args = body.trim().split(/ +/).slice(1);
    const query = q = args.join(' ');
    const query1 = q1 = query.split('|')[0];
    const query2 = q2 = query.split('|')[1];
    const quoted = msg.quoted ? msg.quoted : msg;

    if (!config.public_mode) {
      if (!msg.key.fromMe) {
        return;
      };
    };

    if (msg.message) {
      client.readMessages([msg.key]);

      console.log(
        chalk.bgMagenta(' [New Message] '),
        chalk.cyanBright('Time: ') + chalk.greenBright(new Date()) + '\n',
        chalk.cyanBright('Message: ') + chalk.greenBright(budy || msg.mtype) + '\n' +
        chalk.cyanBright('From:'), chalk.greenBright(msg.pushName), chalk.yellow('- ' + msg.sender.split('@')[0]) + '\n' +
        chalk.cyanBright('Chat Type:'), chalk.greenBright(!msg.isGroup ? 'Private Chat - ' + chalk.yellow(client.user.id.split(':')[0]) : 'Group Chat - ' + chalk.yellow(msg.chat.split('@')[0]))
      );
    };

    if (!body.startsWith(config.prefix) || body === config.prefix) {
      return;
    };

    switch (command) {
      case 'test': {
        msg.reply('Ok, Success!');
        break;
      };
      
      case 'react': {
        const emoji = query ? query : '🗿';
        
        client.sendMessage(msg.chat, {
          react: {
            text: emoji,
            key: msg.key,
          },
        });
        break;
      };
      
      case 'text': {
        client.sendMessage(msg.chat, {
          text: 'Text'
        }, {
          quoted: quoted
        });
        break;
      };
      
      case 'document': {
        client.sendMessage(msg.chat, {
          document: {
            url: 'https://cdn.danitechno.com/daniapi/img/banner.jpeg'
          },
          fileName: 'example.jpeg',
          mimetype: 'image/jpeg'
        }, {
          quoted: quoted
        });
        break;
      };
      
      case 'image': {
        client.sendMessage(msg.chat, {
          image: {
            url: 'https://cdn.danitechno.com/daniapi/img/banner.jpeg'
          },
          caption: 'Image'
        }, {
          quoted: quoted
        });
        break;
      };
      
      case 'video': {
        client.sendMessage(msg.chat, {
          video: {
            url: 'https://dtubein.danitechno.com/uploads/videos/lv_7175020052696091906_20230628222944_64b4e41f00726.mp4'
          },
          caption: 'Video'
        }, {
          quoted: quoted
        });
        break;
      };
      
      case 'audio': {
        client.sendMessage(msg.chat, {
          audio: {
            url: 'https://cdn.danitechno.com/audio/dj-joanna-breakbeat.mp3'
          },
          mimetype: 'audio/mpeg',
          ptt: false
        }, {
          quoted: quoted
        });
        break;
      };
      
      case 'voice': {
        client.sendMessage(msg.chat, {
          audio: {
            url: 'https://cdn.danitechno.com/audio/dj-joanna-breakbeat.mp3'
          },
          mimetype: 'audio/mpeg',
          ptt: true
        }, {
          quoted: quoted
        });
        break;
      };
      
      default: {
        msg.reply(`Command: *${config.prefix}${command}*, tidak tersedia!`);
      };
    };
  } catch (error) {
    msg.reply('Terjadi kesalahan pada server.');
    console.error(error);
  };
};
```

### Contoh kode/skrip bot WhatsApp lengkap
<a href="https://github.com/danitechnology/example-wa-bot-script">Klik disini</a> untuk mengunduh proyek.

## Informasi
* Pembuat / Pengembang: Dani Technology (Dani Ramdani) - FullStack Engineer
* Kontak Pembuat / Pengembang: 0895 3416 24750 (WhatsApp), contact@danitechno.com (Email)

## Terimakasih Kepada
* Dani Technnology (Dani Ramdani) - FullStack Engineer (Pembuat / Pengembang)
* @whiskeysockets/baileys (Penyedia Library "Baileys")