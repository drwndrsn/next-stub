# FakeNext
Unit test middleware by replacing next() with a callback

```javascript
const fakenext = require('fakenext')
const chai = require('chai'),
    expect = chai.expect

chai.config.includeStack = true
 
let req = {body: {user: "bob"}}
    res = {locals: {}}
 
let middlewareBob = function (req, res, next) {
    res.locals.user = req.body.user.toUpperCase()
    next()
}

let middlewareErr = function (req, res, next) {
    if (req.body.user !== "bleh") {
        next(Error('bob is not bleh'))
    }
}

describe('middleware next() is faked', () => {
    it('req and res work when next called with no arg', () => {

        fakenext(middlewareBob, req, res, function (err, req, res) {
            expect(res.locals.user).to.equal('BOB')
            expect(err).to.not.exist
        })

    })

    it('err handled if arg passed to next', () => {

        fakenext(middlewareErr, req, res, function (err, req, res) {
            expect(err.message).to.match(/bob is not bleh/)
        })

    })
})
```
