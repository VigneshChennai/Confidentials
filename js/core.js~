
// In the following line, you should include the prefixes of implementations you want to test.
window.indexedDB = window.indexedDB || window.mozIndexedDB || window.webkitIndexedDB || window.msIndexedDB;
// DON'T use "var indexedDB = ..." if you're not in a function.
// Moreover, you may need references to some window.IDB* objects:
window.IDBTransaction = window.IDBTransaction || window.webkitIDBTransaction || window.msIDBTransaction;
window.IDBKeyRange = window.IDBKeyRange || window.webkitIDBKeyRange || window.msIDBKeyRange;
// (Mozilla has never prefixed these objects, so we don't need window.mozIDB*)



core = {};

core.version = "1";

//the following value will be encrypted by the user password on user creation and will be stored in db
//During authentication, the value in the db will be decrypted using the user entered password and decrypted value 
//will be compared with the following value. 

core.validatePassword = "lakyr2 b9pq8rycn3468ot57nadhsfiuxo,;oeuiarh,vnjdkmsclufn3ol579y3towgaerhgsd'kdufkt2y[340ctmgsdhja";

core.User = function(name, email, password) {
    this.name = name;
    this.email = email;
    this.password = password;
}

core.Confidential = function (groupid, email, key, value) {
    this.groupid = groupid;
    this.email = email;
    this.key = key;
    this.value = value;
}

core.Group = function(userid, gname) {
    this.userid = userid;
    this.gname = gname;
}

core.init = function() {
    var dbrequest = indexedDB.open("Confidentials", this.version);

    dbrequest.onupgradeneeded = function(event) {
        var db = event.target.result;
        var objectStore = db.createObjectStore("users", { keyPath: "id", autoIncrement: true });
        objectStore.createIndex("name", "name", { unique: false });
        objectStore.createIndex("email", "email", { unique: true });
        
        objectStore = db.createObjectStore("confidentials", { keyPath: "id", autoIncrement: true });
        objectStore.createIndex("userid", "userid", { unique: false });
        objectStore.createIndex("key", "key", { unique: false });

        objectStore = db.createObjectStore("groups", { keyPath: "id", autoIncrement: true });
        objectStore.createIndex("userid", "userid", { unique: false });
        objectStore.createIndex("gname", "gname", { unique: false });
        
        alert("New schema created");
    }
}


core.createUser = function(user) {
    var result = new common.Request();
    
    var request = indexedDB.open("Confidentials", this.version);
    request.onsuccess = function(event) {
        var db = event.target.result;
        var trans = db.transaction(['users'], "readwrite");
        var store = trans.objectStore('users');
        var passBack = user.password;
        user.password = sjcl.encrypt(user.password, core.validatePassword);
        var req = store.put(user);
        
        req.onsuccess = function (event) {
            user.password = passBack;
            var event = new common.Event(result, user);
            result.raiseEvent("success",event);
        }
        
        req.onerror = function (event) {
            var event = new common.Event(result, []);
            result.raiseEvent("error",event);
        }
        
    }
    request.onerror = function(event) {
        var db = event.target.result;
        var event = new common.Event(result, []);
        result.raiseEvent("error",event);
    }
    return result;
}

core.getUsers = function(callback) {
    var users = [];
    var request = indexedDB.open("Confidentials", this.version);
    request.onsuccess = function(event) {
        var db = event.target.result;
        var trans = db.transaction(['users'], "readwrite");
        var store = trans.objectStore('users');
        var req = store.openCursor();
        req.onsuccess = function (event) {
            var result = event.target.result;
            if(result) {
                users.push(result.value);
                result.continue();
            }else {
                callback(users);
            }
        }
        req.onerror = function (event) {
            alert("Error in creation user");
        }
    }
    request.onerror = function(event) {
        var db = event.target.result;
    }
}
    
/*
    var login = function(email, passwd) {
        var request = indexedDB.open("Confidentials", version);
        request.onsuccess = function(event) {
            var db = event.target.result;
            var trans = db.transaction(["users"]);
            var objectStore = trans.objectStore("users");
            var emailindex = objectStore.index("email");
            var request = emailindex.get(email);
            var returnreq;
            request.onsuccess = function (event) {
                var user = event.target.result;
                if(validatePassword == sjcl.decrypt(passwd, user.password)) {
                    alert("Login successful");
                    user.password = passwd;
                    if(returnreq.onsuccess) {
                        returnreq.onsuccess(user);
                    }
                } else {
                    alert("Invalid email or password");
                    if(returnreq.onfailure) {
                            returnreq.onfailure();  
                    }
                }
            }
            request.onerror = function(event) {
                alert("Invalid email or password");
                if(returnreq.onfailure) {
                        returnreq.onfailure();  
                }
            }
            return returnreq;
        }
    }        
 */
 core.init();
