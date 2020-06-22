### Feature
base on eosjs v20.0.3. 
`transact()` added a new optionally parameter: `refIrreversibleBlock`
when it set to `true`, will let eosjs ref last irreversible block
and may prevent "Invalid Reference Block" error

the origin v20.0.3 is use `(head_block - blocksBehind)`. this may cause "Invalid Reference Block" error when your ref block been rollback (but more secure on relay attack)

### Sending a transaction

Same as origin `transact()`, but you can set `refIrreversibleBlock: true` to use 
last irreversible block as ref block. if no set `expireSecond` it will default as `60 * 60` seconds.

```js
(async () => {
  const result = await api.transact({
    actions: [{
      account: 'eosio.token',
      name: 'transfer',
      authorization: [{
        actor: 'useraaaaaaaa',
        permission: 'active',
      }],
      data: {
        from: 'useraaaaaaaa',
        to: 'useraaaaaaab',
        quantity: '0.0001 SYS',
        memo: '',
      },
    }]
  }, {
    // blocksBehind: 3,
    // expireSeconds: 30,
    refIrreversibleBlock: true,
  });
  console.dir(result);
})();
```

### Error handling (same as origin)

use `RpcError` for handling RPC Errors
```js
...
try {
  const result = await api.transact({
  ...
} catch (e) {
  console.log('\nCaught exception: ' + e);
  if (e instanceof RpcError)
    console.log(JSON.stringify(e.json, null, 2));
}
...
```

## License

[MIT](./LICENSE)

## Important

See LICENSE for copyright and license terms.  Block.one makes its contribution on a voluntary basis as a member of the EOSIO community and is not responsible for ensuring the overall performance of the software or any related applications.  We make no representation, warranty, guarantee or undertaking in respect of the software or any related documentation, whether expressed or implied, including but not limited to the warranties or merchantability, fitness for a particular purpose and noninfringement. In no event shall we be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or documentation or the use or other dealings in the software or documentation.  Any test results or performance figures are indicative and will not reflect performance under all conditions.  Any reference to any third party or third-party product, service or other resource is not an endorsement or recommendation by Block.one.  We are not responsible, and disclaim any and all responsibility and liability, for your use of or reliance on any of these resources. Third-party resources may be updated, changed or terminated at any time, so the information here may be out of date or inaccurate.
