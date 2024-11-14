import { Transfer as TransferEvent } from "../generated/Contract/Contract";
import { Transfer } from "../generated/schema";
import { crypto, Bytes } from "@graphprotocol/graph-ts";

export function handleTransfer(event: TransferEvent): void {
  // Create a unique ID by hashing the combined string, and cast as Bytes
  let id = crypto.keccak256(Bytes.fromUTF8(event.transaction.hash.toHex() + "-" + event.logIndex.toString()));

  // Convert the Bytes ID into a string
  let idString = id.toHex();

  let entity = new Transfer(idString);

  entity.from = event.params.from;
  entity.to = event.params.to;
  entity.value = event.params.value;

  entity.blockNumber = event.block.number;
  entity.blockTimestamp = event.block.timestamp;
  entity.transactionHash = event.transaction.hash;

  entity.save();
}
