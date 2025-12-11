# Presenter Checklist - Jenkins to GitHub Actions Migration Demo

Use this checklist to ensure a smooth, professional presentation.

---

## 📅 1 Week Before Presentation

### Materials Review
- [ ] Read DEMO_GUIDE.md thoroughly
- [ ] Review PRESENTER_SCRIPT.md
- [ ] Familiarize yourself with QUICK_REFERENCE.md
- [ ] Review sample Jenkinsfiles in demos/ directory
- [ ] Read TROUBLESHOOTING.md for common issues

### Technical Setup
- [ ] Verify Jenkins instance is accessible
- [ ] Generate Jenkins API token
- [ ] Generate GitHub Personal Access Token (with `workflow` scope)
- [ ] Create test GitHub repository for Demo 5
- [ ] Test network connectivity to Jenkins from presentation machine
- [ ] Ensure firewall/VPN settings allow Jenkins access

### Slide Deck
- [ ] Create slides based on SLIDE_OUTLINE.md
- [ ] Add company branding
- [ ] Include relevant success stories
- [ ] Add presenter contact information
- [ ] Export backup PDF version

### Practice
- [ ] Do complete dry run (60 minutes)
- [ ] Time each demo section
- [ ] Practice transitions between demos
- [ ] Test all commands in terminal
- [ ] Identify potential failure points
- [ ] Prepare backup plans for each demo

---

## 📅 1 Day Before Presentation

### Software Installation
- [ ] Install/update Docker Desktop
  ```bash
  docker --version  # Should be 24.x or higher
  docker ps  # Verify it's running
  ```
- [ ] Install/update GitHub CLI
  ```bash
  gh --version  # Should be 2.x or higher
  gh auth status  # Verify authentication
  ```
- [ ] Install GitHub Actions Importer
  ```bash
  gh extension install github/gh-actions-importer
  gh actions-importer version
  ```
- [ ] Update all tools to latest versions
  ```bash
  gh extension upgrade gh-actions-importer
  ```

### Credentials Preparation
- [ ] Write down Jenkins URL (no trailing slash)
- [ ] Copy Jenkins username to notes
- [ ] Copy Jenkins API token to secure notes
- [ ] Copy GitHub PAT to secure notes
- [ ] Test Jenkins credentials
  ```bash
  curl -u username:token http://jenkins/api/json
  ```
- [ ] Test GitHub authentication
  ```bash
  gh auth status
  ```

### Environment Setup
- [ ] Clean terminal history: `history -c`
- [ ] Set terminal font size to 18-20pt
- [ ] Choose high-contrast terminal theme
- [ ] Disable terminal notifications
- [ ] Close unnecessary applications
- [ ] Charge laptop fully
- [ ] Test presentation mode/screen mirroring

### Content Preparation
- [ ] Clone this repository to presentation machine
- [ ] Create working directory: `mkdir ~/demo-workspace`
- [ ] Prepare text file with:
  - Jenkins URLs to use
  - GitHub repository URLs
  - Secret values (if safe to store)
  - Backup commands

### Backup Plans
- [ ] Take screenshots of successful command outputs
- [ ] Record backup video of each demo (optional but recommended)
- [ ] Save example audit/forecast reports
- [ ] Prepare "what to do if..." scenarios
- [ ] Have mobile hotspot ready if WiFi fails

---

## 📅 2 Hours Before Presentation

### Final Technical Checks
- [ ] Verify Docker is running
  ```bash
  docker ps
  ```
- [ ] Verify GitHub CLI authentication
  ```bash
  gh auth status
  gh auth refresh
  ```
- [ ] Test Jenkins connectivity
  ```bash
  curl -I http://jenkins
  ```
- [ ] Pre-configure Actions Importer (optional, to save demo time)
  ```bash
  gh actions-importer configure
  ```

### Workspace Setup
- [ ] Open terminal at 18pt+ font size
- [ ] Open QUICK_REFERENCE.md in browser tab
- [ ] Open TROUBLESHOOTING.md in browser tab
- [ ] Open presentation slides
- [ ] Open text file with URLs and commands
- [ ] Position windows for easy switching
- [ ] Test screen sharing if virtual presentation

### Room/Audio Setup (In-Person)
- [ ] Test projector/screen
- [ ] Verify resolution (1080p recommended)
- [ ] Test audio/microphone
- [ ] Position laptop for easy view and typing
- [ ] Have water nearby
- [ ] Have charging cable plugged in

### Virtual Setup (Online)
- [ ] Test screen sharing
- [ ] Test audio/video
- [ ] Close unnecessary applications
- [ ] Disable notifications (Slack, email, etc.)
- [ ] Set status to "Presenting" or "Do Not Disturb"
- [ ] Have backup device ready

### Personal Preparation
- [ ] Review key talking points
- [ ] Practice opening and closing
- [ ] Prepare for Q&A
- [ ] Set phone timer for 60 minutes
- [ ] Take a bathroom break
- [ ] Get water/coffee
- [ ] Relax and breathe! 🧘

---

## 📅 Just Before Starting (5 Minutes)

### Final Checks
- [ ] Docker running: `docker ps`
- [ ] Terminal font readable on projection
- [ ] Slides loaded and ready
- [ ] QUICK_REFERENCE.md open in background
- [ ] All credentials ready to paste
- [ ] Working directory created and empty
- [ ] Phone on silent
- [ ] Laptop plugged in
- [ ] Water within reach

### Mental Prep
- [ ] Take three deep breaths
- [ ] Review first 2 minutes of script
- [ ] Remember: You've got this! 💪
- [ ] Smile and show enthusiasm

---

## ⏱️ During Presentation

### Opening (0:00-5:00)
- [ ] Welcome audience warmly
- [ ] Show agenda slide
- [ ] Do audience poll (show of hands)
- [ ] Set expectations for timing
- [ ] Encourage questions throughout

### Demo 1: Setup (5:00-15:00)
- [ ] Switch to terminal clearly
- [ ] Explain what you're doing before typing
- [ ] Show command output clearly
- [ ] Point out key information
- [ ] Return to slides for summary
- [ ] Check: Any questions?

### Demo 2: Audit (15:00-25:00)
- [ ] Switch to terminal
- [ ] Run audit command
- [ ] Explain while it runs (don't wait silently)
- [ ] Walk through report sections
- [ ] Highlight key insights
- [ ] Return to slides for summary
- [ ] Check: Any questions?

### Demo 3: Forecast (25:00-35:00)
- [ ] Switch to terminal
- [ ] Run forecast command
- [ ] Discuss costs while running
- [ ] Walk through cost breakdown
- [ ] Emphasize ROI
- [ ] Return to slides for summary
- [ ] Check: Any questions?

### Demo 4: Dry Run (35:00-47:00)
- [ ] Show original Jenkinsfile
- [ ] Run dry-run command
- [ ] Compare before/after
- [ ] Review migration notes
- [ ] Discuss manual work needed
- [ ] Return to slides for summary
- [ ] Check: Any questions?

### Demo 5: Migration (47:00-60:00)
- [ ] Run migrate command
- [ ] Show pull request in browser
- [ ] Configure secrets
- [ ] Merge and run workflow
- [ ] Show successful run
- [ ] Celebrate success! 🎉
- [ ] Return to slides for summary
- [ ] Check: Any questions?

### Closing & Q&A (60:00-65:00)
- [ ] Show summary slide
- [ ] Recap key takeaways
- [ ] Share resources
- [ ] Open floor for questions
- [ ] Thank audience
- [ ] Share contact information

### Throughout
- [ ] Make eye contact with audience
- [ ] Check time every 10 minutes
- [ ] Adjust pace if running over
- [ ] Stay energetic and enthusiastic
- [ ] Don't apologize for minor mistakes
- [ ] Engage with questions positively

---

## 📋 If Something Goes Wrong

### Docker Issues
- [ ] Show screenshot of working docker
- [ ] Explain the concept without demo
- [ ] Continue to next demo
- [ ] OR switch to backup recording

### Network Issues
- [ ] Switch to mobile hotspot
- [ ] Use cached/pre-run results
- [ ] Show screenshots
- [ ] Explain what would happen

### Command Fails
- [ ] Stay calm - this happens!
- [ ] Check TROUBLESHOOTING.md
- [ ] Try once more with verbose flag
- [ ] If still failing, show screenshot of working version
- [ ] Explain what should happen
- [ ] Move on - don't get stuck

### Running Over Time
- [ ] Skip detailed walkthrough of Demo 4
- [ ] Summarize forecast results quickly
- [ ] Cut Q&A short with "contact me after"
- [ ] Share resources and wrap up

### Technical Jargon Questions
- [ ] Ask them to hold thought
- [ ] Add to parking lot
- [ ] Address at end if time
- [ ] Offer to follow up via email

---

## 📅 After Presentation

### Immediate Follow-Up (Same Day)
- [ ] Share slides with attendees
- [ ] Share link to this repository
- [ ] Send thank you email
- [ ] Share any resources promised during Q&A
- [ ] Note any questions you couldn't answer

### Within 1 Week
- [ ] Follow up on unanswered questions
- [ ] Offer 1:1 help for those interested
- [ ] Share recording if available
- [ ] Ask for feedback
- [ ] Update materials based on feedback

### Documentation
- [ ] Note what worked well
- [ ] Note what needs improvement
- [ ] Record timing of each section
- [ ] Document any issues encountered
- [ ] Update this checklist for next time

### Personal Review
- [ ] What went well?
- [ ] What could be improved?
- [ ] Were demos clear?
- [ ] Was timing good?
- [ ] How was audience engagement?
- [ ] Would I change anything?

---

## 🎯 Success Metrics

A successful presentation includes:
- ✅ Completed all 5 demos
- ✅ Stayed within 60-65 minutes
- ✅ Audience asked questions (engagement)
- ✅ Clear explanations of each concept
- ✅ Showed real, working examples
- ✅ Provided actionable next steps
- ✅ Shared resources for follow-up

---

## 📝 Quick Reference Commands

Have these ready to copy-paste:

```bash
# Installation
gh extension install github/gh-actions-importer

# Configuration  
gh actions-importer configure

# Audit
gh actions-importer audit jenkins --output-dir ./audit

# Forecast
gh actions-importer forecast jenkins --output-dir ./forecast

# Dry Run
gh actions-importer dry-run jenkins \
  --source-url http://jenkins/job/my-job \
  --output-dir ./dry-run

# Migrate
gh actions-importer migrate jenkins \
  --source-url http://jenkins/job/my-job \
  --target-url https://github.com/org/repo \
  --output-dir ./migration

# Verify installations
docker --version
gh --version
gh actions-importer version

# Test connectivity
docker ps
gh auth status
curl -I http://jenkins
```

---

## 🆘 Emergency Contacts

Before presentation, identify:
- [ ] IT support contact (for technical issues)
- [ ] Network team (for connectivity issues)
- [ ] Backup presenter (if available)
- [ ] Room tech support (for in-person events)
- [ ] Event organizer contact

---

## 💡 Pro Tips

### Energy Management
- 🌟 Start with high energy
- ☕ Have caffeine 30 min before, not during
- 💧 Drink water between demos
- 😊 Smile - it shows in your voice
- 🎯 Focus on helping the audience, not perfection

### Handling Questions
- ✅ Repeat the question for everyone
- ✅ Validate the questioner: "Great question!"
- ✅ Be honest if you don't know
- ✅ Offer to follow up via email
- ✅ Keep answers concise (2 min max)

### Technical Tips
- 💻 Type slowly and deliberately
- 🗣️ Say what you're typing as you type
- ⏸️ Pause after important points
- 👁️ Check output before moving on
- 🔄 Don't be afraid to restart if needed

### Presentation Skills
- 👥 Make eye contact (camera for virtual)
- 🎭 Use hand gestures naturally
- 📢 Vary your tone and pace
- 🎬 Tell stories when relevant
- 🎉 Celebrate successes ("Look at that!")

---

## 📞 Post-Presentation Support

Be prepared to help attendees after:

- [ ] Offer 30-minute consultation sessions
- [ ] Create Slack/Teams channel for questions
- [ ] Share internal documentation
- [ ] Schedule follow-up workshop if interest is high
- [ ] Connect interested parties with each other

---

**Remember:** 
- Preparation beats perfection
- Technical glitches happen to everyone
- Your enthusiasm is contagious
- Focus on helping, not impressing
- You've got all the materials you need

**You're going to do great! 🚀**

---

**Checklist Version:** 1.0  
**Last Updated:** December 2024  
**Print this and keep it handy during your presentation!**
