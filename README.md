const axios = require("axios");
const readline = require("readline-sync");
const moment = require("moment");

var token = readline.question(`[ ${moment().format("HH:mm:ss")} ] Token : `);

const url = "https://api-backend.yescoin.gold/game/collectCoin";

const headers = {
  Accept: "application/json, text/plain, */*",
  "Accept-Encoding": "gzip, deflate, br, zstd",
  "Accept-Language": "en-US,en;q=0.9",
  "Cache-Control": "no-cache",
  "Content-Length": "1",
  "Content-Type": "application/json",
  Origin: "https://www.yescoin.gold",
  Pragma: "no-cache",
  Referer: "https://www.yescoin.gold/",
  "Sec-Ch-Ua":
    '"Chromium";v="124", "Microsoft Edge";v="124", "Not-A.Brand";v="99", "Microsoft Edge WebView2";v="124"',
  "Sec-Ch-Ua-Mobile": "?0",
  "Sec-Ch-Ua-Platform": '"Windows"',
  "Sec-Fetch-Dest": "empty",
  "Sec-Fetch-Mode": "cors",
  "Sec-Fetch-Site": "same-site",

  Token: token,
  "User-Agent":
    "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiI5OTMwNTU3NTIiLCJjaGF0SWQiOiI5OTMwNTU3NTIiLCJpYXQiOjE3MjUwNDAyMTgsImV4cCI6MTcyNzYzMjIxOCwicm9sZUF1dGhvcml6ZXMiOltdLCJ1c2VySWQiOjE3ODIyNDA1OTc1MTY0NDM2NDh9.EQdWeJDBHe8pELMT4hL6Jceaur700kQy81DvjMGgr08JhFPqfmH9Yy7lccTlLcFLESXa6gdxYtiuB4q4lBE8Jw",
};

const data = 9;

function postRequest() {
  axios
    .post(url, data, { headers })
    .then((response) => {
      const { message, data } = response.data;
      if (message !== "Success") {
        setTimeout(() => {
          postRequest();
        }, 60000);
        console.log(
          `[ ${moment().format("HH:mm:ss")} ] Energy 0 Delay 1 Minute...`,
        );
      } else {
        console.log(
          `[ ${moment().format("HH:mm:ss")} ] Click point : ${
            data.collectAmount
          }`,
        );
        postRequest();
      }
    })
    .catch((error) => {
      console.error("Error:", error);
      postRequest();
    });
}

postRequest();
