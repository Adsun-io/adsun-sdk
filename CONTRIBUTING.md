# Contributing

- JavaScript package: `cd packages/js && npm test`
- Python package: `cd packages/python && python -m unittest discover -s tests`

`packages/js/sdk/adsun.min.js` is a snapshot of the script served at https://api.adsun.io/sdk.js. The loader always
uses the live URL by default, so publishers get server-matched updates without upgrading the npm package.
