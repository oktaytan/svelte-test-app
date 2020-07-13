<script>
  import PollStore from "../stores/PollStore.js";
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  export let poll;
  import { tweened } from "svelte/motion";

  // reactive values
  $: totalVotes = poll.votesA + poll.votesB;
  $: percentA = Math.floor((100 / totalVotes) * poll.votesA) || 0;
  $: percentB = Math.floor((100 / totalVotes) * poll.votesB) || 0;

  // tweened percentages
  const tweenedA = tweened(0);
  const tweenedB = tweened(0);
  $: tweenedA.set(percentA);
  $: tweenedB.set(percentB);

  // handling votes
  const handleVote = (option, id) => {
    PollStore.update(currentPolls => {
      let copiedPolls = [...currentPolls];
      let upvotedPoll = copiedPolls.find(poll => poll.id === id);

      switch (option) {
        case "a":
          upvotedPoll.votesA++;
          break;
        case "b":
          upvotedPoll.votesB++;
          break;
        default:
          break;
      }

      return copiedPolls;
    });
  };

  // Delete Poll
  const handleDelete = id => {
    PollStore.update(currentPolls => {
      return currentPolls.filter(poll => poll.id != id);
    });
  };
</script>

<style>
  h3 {
    margin: 0 auto;
    color: #555;
  }
  p {
    margin-top: 6px;
    font-size: 14px;
    color: #aaa;
    margin-bottom: 30px;
  }
  .answer {
    background: #fafafa;
    cursor: pointer;
    margin: 10px auto;
    position: relative;
  }
  .answer:hover {
    opacity: 0.6;
  }
  span {
    display: inline-block;
    padding: 10px 20px;
    user-select: none;
  }
  .percent {
    height: 100%;
    position: absolute;
    box-sizing: border-box;
  }
  .percent-a {
    background: rgba(217, 27, 66, 0.2);
    border-left: 4px solid crimson;
  }
  .percent-b {
    background: rgba(69, 196, 150, 0.2);
    border-left: 4px solid #12ca7e;
  }

  .delete {
    margin-top: 2rem;
    text-align: center;
  }
</style>

<Card>
  <div class="poll">
    <h3>{poll.question}</h3>
    <p>Total votes: {totalVotes}</p>
    <div class="answer" on:click={() => handleVote('a', poll.id)}>
      <div class="percent percent-a" style="width: {$tweenedA}%;" />
      <span>{poll.answerA} ({poll.votesA} votes)</span>
    </div>
    <div class="answer" on:click={() => handleVote('b', poll.id)}>
      <div class="percent percent-b" style="width: {$tweenedB}%;" />
      <span>{poll.answerB} ({poll.votesB} votes)</span>
    </div>
    <div class="delete">
      <Button type="primary" flat={true} on:click={() => handleDelete(poll.id)}>
        Delete
      </Button>
    </div>
  </div>
</Card>
