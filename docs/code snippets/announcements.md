# Ordinary & Internal Components

Announcements are a one way communication method sent from server to client.
They can be 
1. Specific to a player 
2. Announced to a whole server
3. Globally announced to all running servers (centurion implementation to be added)

It's recommended not to put too much text on one announcement and separate them into shorter texts because they might cover the whole screen

## Example

![announcement ss 1](arcana-xvi-docs/img/announcements/announcement_1.png)

```ts
// src/server/services/AnnouncerTestService.ts
import { Service, OnStart, Dependency } from "@flamework/core";
import { AnnouncerService } from "./AnnouncerService"; // Import the service
import { PlayerService } from "./PlayerService";

@Service()
export class AnnouncerTestService implements OnStart {
	constructor(private readonly playerService: PlayerService, private readonly announcerService: AnnouncerService) {}

	onStart() {
		// Connect to PlayerAdded event to send announcements when a player joins
		this.playerService.playerJoined.Connect((player) => {
			this.handlePlayerJoined(player);
		});

		// Also send announcements to any players already in the game when the server starts
		this.playerService.getPlayers().forEach((player) => {
			this.handlePlayerJoined(player);
		});
	}

	private handlePlayerJoined(player: Player) {
		print(`Player ${player.Name} joined. Triggering test announcements.`);

		this.announcerService.announcePlayer(
			player,
			`<font color="##632c12">Dev Will</font>`,
			`<b>after i applied some minor fixes, theres an error</b>
			"Object literal may only specify known properties, and 'Transparency' does not exist in type 'Instance'" on the line "fadeOutTween = TweenService.Create(
			{ Transparency: dialogTransparency }..."
			 this argument is supposed to accept the instance that will be tweened. as in the textlabel, imagelabel, or frame's transparency `,
		);

		// --- Test Case 1: Basic Welcome Announcement with Rich Text ---
		// Uses AnnouncePlayer to target only the joining player.
		this.announcerService.announcePlayer(
			player,
			'<font color="#9e2e1c">Arve</font>',
			`Greetings, <b>${player.Name}</b>! Kill yourself!`,
		);

		// --- Test Case 2: Important Info with Key and Spaced Text (and shake effect) ---
		// Uses a task.wait to space out the announcements, making them easier to observe.
		this.announcerService.announcePlayer(
			player,
			`<font color="#400f0d">Dev Adrian</font>`,
			`<sh>This is for P1</sh>,`,
		);

		// --- Test Case 3: Global Server Announcement (uses MessagingService) ---
		// This will attempt to send to all active servers.
		// It requires MessagingService to be enabled for your game and might not show if you're only in Play Solo.
		// To see it in action, you'd typically need multiple studio instances or a live game server.
		this.announcerService.announceGlobal("XXX", `This is a global announcement`);

		// --- Test Case 4: Mixed Rich Text and Headers ---
		this.announcerService.announcePlayer(player, "uh. yeah", `wawawawa`);
	}
}

```
