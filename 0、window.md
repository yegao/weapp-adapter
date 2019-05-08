**先看一下这一部分的代码**
```javascript
(function (module, exports, __webpack_require__) {
    'use strict'
    var _window2 = __webpack_require__(1)
    var _window = requireWildcard(_window2)
    function requireWildcard(obj) {
        if (obj && obj.__esModule) {
            return obj
        } else {
            var newObj = {};
            if (obj != null) {
                for (var key in obj) {
                    if (Object.prototype.hasOwnProperty.call(obj, key)) newObj[key] = obj[key]
                }
            }
            newObj.default = obj;
            return newObj
        }
    }
    var global = GameGlobal
    function inject() {
        _window.addEventListener = _window.canvas.addEventListener = function (type, listener) {
            _window.document.addEventListener(type, listener)
        }
        _window.removeEventListener = _window.canvas.removeEventListener = function (type, listener) {
            _window.document.removeEventListener(type, listener)
        }
        var _wx$getSystemInfoSync = wx.getSystemInfoSync(),
            platform = _wx$getSystemInfoSync.platform
        // 开发者工具无法重定义 window
        if (typeof __devtoolssubcontext === 'undefined' && platform === 'devtools') {
            for (var key in _window) {
                var descriptor = Object.getOwnPropertyDescriptor(global, key)
                if (!descriptor || descriptor.configurable === true) {
                    Object.defineProperty(window, key, {
                        value: _window[key]
                    })
                }
            }
            for (var _key in _window.document) {
                var _descriptor = Object.getOwnPropertyDescriptor(global.document, _key)
                if (!_descriptor || _descriptor.configurable === true) {
                    Object.defineProperty(global.document, _key, {
                        value: _window.document[_key]
                    })
                }
            }
            window.parent = window
        } else {
            for (var _key2 in _window) {
                global[_key2] = _window[_key2]
            }
            global.window = _window
            window = global
            window.top = window.parent = window
        }
    }
    if (!GameGlobal.__isAdapterInjected) {
        GameGlobal.__isAdapterInjected = true
        inject()
    }
})
```