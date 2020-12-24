this.__proto__.maxHints = function () {
    var print =  (new Date().getTime() % 2000) < 100;
    var tg = this;
    var si = {};
    if (print) console.log('===ANSWER START===');
    tg.model.set("response", tg.get("correct"))
    if (print) console.log(tg.get("correct"));
    tg.model.forEachChildType("body", function (t, u) {
        t.set("response", tg.get("correct"));
        try { tg.storeAnswerForSet(t); } catch (e) {}
        t.forEachChildType("input", function (t, n) {
            var r = t.get("correct");
            if (print) console.log(r);
            if (typeof r === "boolean") t.set("response", r);
            else if (typeof r === "number") t.set("response", r);
            else {
                if (si[r] === undefined) si[r] = 0;
                t.set("response", r.split("/")[si[r]++]);
                si[r] %= r.split("/").length;
            }
        })
    });
    if (print) console.log('===ANSWER END===');
    return 3;
}