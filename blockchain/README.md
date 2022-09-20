# Blockchain Test

## Duplicate Transaction

### Ethereum

The nonce is returned from transaction noncer. The nonce can be got by account address. The transaction has nonce field and the account keeps track of the nonce. They are comparatable. The account nonce is the transaction nonce plus one. The nonce value is passed as input parameter when creating new transaction. 

There are three important files: transaction.go, tx_pool.go, tx_noncer.go. All of them are related to nonce.

```go
	// Ensure the transaction adheres to nonce ordering
	if pool.currentState.GetNonce(from) > tx.Nonce() {
		return ErrNonceTooLow
	}
```

### Fabric

The nonce is included in the signature header. The txutils file computes the transaction id by combining nonce and creator. The nonce is generated from crypto package. The transaction id is checked and validated in the transaction flow. The header contains two parts: channel header and signature header. The transaction id is in the channel header. The nonce and creator data are in the signature header. Their equality can be validated. Both of message and transaction are validated in the transaction process.

```go

// CheckTxIdDupsLedger returns a vlockValidationResult enhanced with the respective
// error codes if and only if there is transaction with the same transaction identifier
// in the ledger or no decision can be made for whether such transaction exists;
// the function returns nil if it has ensured that there is no such duplicate, such
// that its consumer can proceed with the transaction processing
func (v *TxValidator) checkTxIdDupsLedger(tIdx int, chdr *common.ChannelHeader, ldgr ledger.PeerLedger) (errorTuple *blockValidationResult) {

	// Retrieve the transaction identifier of the input header
	txID := chdr.TxId

	// Look for a transaction with the same identifier inside the ledger
	_, err := ldgr.GetTransactionByID(txID)

	// if returned error is nil, it means that there is already a tx in
	// the ledger with the supplied id
	if err == nil {
		logger.Error("Duplicate transaction found, ", txID, ", skipping")
		return &blockValidationResult{
			tIdx:           tIdx,
			validationCode: peer.TxValidationCode_DUPLICATE_TXID,
		}
	}

    return nil
}
```
