## Issue in the existing design
1. Tight coupling between bulk-processor and payment-connectors or bulk-connectors.
2. Current implementation has payment_mode condition based on organisation/provider name like SLCB, GSMA, MOJALLOP. Rather it should be more generic like p2p, c2b, collection etc.
3. For GSMA payment_mode it's actually fine that we are calling an p2p API in channel-connector but for SLCB payment_mode we are directly starting the zeebe-workflow which is forcing us to add the SLCB bpmn name in configuration. This also explains the point 1. 


## Solution for payment connectors
We can have the payment_mode in CSV as it is and have a mapping as the configuration which will tell us what type of payment this mode belongs to.
So a mapping would be like `GSMA: transfer`, `MOJALOOP: p2p-transfer`.
