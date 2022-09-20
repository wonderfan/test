# Blockchain Test

## Duplicate Transaction

### Ethereum

The nonce is returned from transaction noncer. The nonce can be got by account address. The transaction has nonce field and the account keeps track of the nonce. They are comparatable. The account nonce is the transaction nonce plus one. The nonce value is passed as input parameter when creating new transaction. 

```go
	// Ensure the transaction adheres to nonce ordering
	if pool.currentState.GetNonce(from) > tx.Nonce() {
		return ErrNonceTooLow
	}
```


