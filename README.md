// ==UserScript==
// @name         Family Farm Script
// @namespace    https:/kem.ooo/
// @version      0.1
// @description  A multi-purpose script for family farm game
// @author       Kemo
// @match        *://farm-us.centurygames.com/*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=centurygames.com
// @grant        unsafeWindow
// @run-at       document-end
// ==/UserScript==

(function () {
    'use strict';

    function print(type, ...args) {
        console[type](`%c Family Farm Script `, `background: #5865f2; color: black; font-weight: bold; border-radius: 5px; `, ...args);
    }

    function whenDefined(object, prop, callback) {
        if (object[prop]) callback();
        else {
            Object.defineProperty(object, prop, {
                configurable: true,
                enumerable: true,
                get: function () {
                    return this['_' + prop];
                },
                set: async function (val) {
                    this['_' + prop] = val;
                    await callback();
                }
            });
        }
    }

    print("log", `Init`);

    whenDefined(unsafeWindow, "Config", async () => {
        unsafeWindow.Log.debug = function() {
            for (var t = [], e = 0; e < arguments.length; e++)
                t[e] = arguments[e];
            var i = "[Debug] " + t.shift();
            console.log.apply(console, [i].concat(t))
        }

        //unsafeWindow.EnvCfg.DEBUG = true;

        whenDefined(unsafeWindow.Config, "data", async () => {
            Object.values(unsafeWindow.Config.Store).forEach(x => {
                if (x.buyable == false) {
                    x.buyable = true;
                }
            });

            ConfirmView.Show("The shop now has everything in the game!\nScript made by: kem0x on github.");
        });
    });

})();
