var common = {};

common.events = {};

common.events.addEventListener = function (name, handler) {
    if (!this.events) this.events = {};
    if (!this.events[name]) this.events[name] = [];
    this.events[name].push(handler);
}

common.events.removeEventListener = function (name, handler) {
    if (!this.events) return;
    if (!this.events[name]) return;
    for (var i = this.events[name].length - 1; i >= 0; i--)
        if (this.events[name][i] == handler)
            this.events[name].splice(i, 1);
}

common.events.raiseEvent = function (name, args) {
    if(typeof(this["on" + name]) == "function") {
             this["on" + name](args);
    }
    if (!this.events) return;
    if (!this.events[name]) return;
    for (var i = 0; i < this.events[name].length; i++)
        this.events[name][i].apply(this, [args,]);
}

common.addEventSupport = function (obj) {
    obj.addEventListener = common.events.addEventListener;
    obj.removeEventListener = common.events.removeEventListener;
    obj.raiseEvent = common.events.raiseEvent;
    return obj;
}

common.Request = function() {
    this.addEventListener = common.events.addEventListener;
    this.removeEventListener = common.events.removeEventListener;
    this.raiseEvent = common.events.raiseEvent;
}

common.Event = function(target, result) {
    this.target = target;
    this.target.result = result;
}
