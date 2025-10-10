---
title: "Major Events in Christian History"
description: "Interactive timeline of Christian history"
lead: "Explore key events in the development of Christianity"
date: 2025-10-09T18:50:00+08:00
lastmod: 2025-10-09T18:50:00+08:00
draft: false
images: []
menu:
  christianity:
    parent: "history"
weight: 999
toc: true
---

## Timeline of Christian History

<div id="timeline-chart" style="width: 100%; height: 800px; margin: 20px 0"></div>

<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
// Function to get text color based on theme
function getTextColor() {
  return document.documentElement.hasAttribute('data-dark-mode') ? '#dee2e6' : '#212529';
}

// Function to get line color based on theme
function getLineColor() {
  return document.documentElement.hasAttribute('data-dark-mode') ? '#6c757d' : '#ccc';
}

// Timeline data
const events = [
  { year: 30, event: "Crucifixion and Resurrection of Jesus", category: "Origin" },
  { year: 50, event: "Council of Jerusalem", category: "Early Church" },
  { year: 64, event: "Persecution under Nero", category: "Persecution" },
  { year: 313, event: "Edict of Milan - Christianity legalized", category: "Legal Recognition" },
  { year: 325, event: "Council of Nicaea", category: "Councils" },
  { year: 380, event: "Christianity becomes Roman state religion", category: "State Religion" },
  { year: 451, event: "Council of Chalcedon", category: "Councils" },
  { year: 1054, event: "Great Schism (East-West Split)", category: "Division" },
  { year: 1095, event: "First Crusade", category: "Crusades" },
  { year: 1517, event: "Protestant Reformation begins", category: "Reformation" },
  { year: 1534, event: "Anglican Church separates from Rome", category: "Reformation" },
  { year: 1545, event: "Council of Trent begins", category: "Counter-Reformation" },
  { year: 1738, event: "Methodist movement begins", category: "Denominations" },
  { year: 1906, event: "Azusa Street Revival (Pentecostalism)", category: "Modern Movements" },
  { year: 1962, event: "Second Vatican Council", category: "Modern Era" }
];

// Set up dimensions
const margin = { top: 60, right: 100, bottom: 70, left: 100 };
const width = document.getElementById('timeline-chart').clientWidth - margin.left - margin.right;
const height = 800 - margin.top - margin.bottom;

// Create SVG container with centering
const container = d3.select("#timeline-chart");
const svgWidth = width + margin.left + margin.right;
const svgHeight = height + margin.top + margin.bottom;

const svg = container
  .append("svg")
  .attr("width", svgWidth)
  .attr("height", svgHeight)
  .style("display", "block")
  .style("margin", "0 auto")
  .append("g")
  .attr("transform", `translate(${margin.left},${margin.top})`);

// Set up scales (Y axis for years now, top to bottom)
const yScale = d3.scaleLinear()
  .domain([0, 2000])
  .range([0, height]);

// Color scale for categories
const colorScale = d3.scaleOrdinal()
  .domain(["Origin", "Early Church", "Persecution", "Legal Recognition", "Councils",
           "State Religion", "Division", "Crusades", "Reformation", "Counter-Reformation",
           "Denominations", "Modern Movements", "Modern Era"])
  .range(d3.schemeSet3);

// Add Y axis
const yAxis = svg.append("g")
  .call(d3.axisLeft(yScale).tickFormat(d => `${d} AD`));

yAxis.selectAll("text")
  .style("font-size", "12px")
  .style("fill", getTextColor());

yAxis.selectAll("path, line")
  .style("stroke", getLineColor());

// Add Y axis label
svg.append("text")
  .attr("transform", "rotate(-90)")
  .attr("x", -height / 2)
  .attr("y", -60)
  .style("text-anchor", "middle")
  .style("font-size", "14px")
  .style("fill", getTextColor())
  .text("Year");

// Create tooltip
const tooltip = d3.select("body")
  .append("div")
  .style("position", "absolute")
  .style("background", "rgba(0, 0, 0, 0.8)")
  .style("color", "white")
  .style("padding", "10px")
  .style("border-radius", "5px")
  .style("pointer-events", "none")
  .style("opacity", 0)
  .style("font-size", "14px")
  .style("max-width", "300px");

// Add vertical timeline line
svg.append("line")
  .attr("x1", width / 2)
  .attr("x2", width / 2)
  .attr("y1", 0)
  .attr("y2", height)
  .attr("stroke", getLineColor())
  .attr("stroke-width", 2);

// Add events
const eventGroups = svg.selectAll(".event")
  .data(events)
  .enter()
  .append("g")
  .attr("class", "event")
  .attr("transform", (d, i) => `translate(${i % 2 === 0 ? width / 2 - 100 : width / 2 + 100}, ${yScale(d.year)})`);

// Add circles for events
eventGroups.append("circle")
  .attr("r", 8)
  .attr("fill", d => colorScale(d.category))
  .attr("stroke", getTextColor())
  .attr("stroke-width", 2)
  .style("cursor", "pointer")
  .on("mouseover", function(event, d) {
    d3.select(this)
      .transition()
      .duration(200)
      .attr("r", 12);

    tooltip
      .style("opacity", 1)
      .html(`<strong>${d.year} AD</strong><br/>${d.event}<br/><em>${d.category}</em>`)
      .style("left", (event.pageX + 10) + "px")
      .style("top", (event.pageY - 10) + "px");
  })
  .on("mouseout", function() {
    d3.select(this)
      .transition()
      .duration(200)
      .attr("r", 8);

    tooltip.style("opacity", 0);
  });

// Add connecting lines from events to timeline
eventGroups.append("line")
  .attr("x1", 0)
  .attr("x2", (d, i) => i % 2 === 0 ? 100 : -100)
  .attr("y1", 0)
  .attr("y2", 0)
  .attr("stroke", d => colorScale(d.category))
  .attr("stroke-width", 2)
  .attr("opacity", 0.5);

// Add event labels (year and event text)
eventGroups.append("text")
  .text(d => `${d.year} AD`)
  .attr("text-anchor", (d, i) => i % 2 === 0 ? "end" : "start")
  .attr("dx", (d, i) => i % 2 === 0 ? -15 : 15)
  .attr("dy", -5)
  .style("font-size", "11px")
  .style("font-weight", "bold")
  .style("fill", getTextColor())
  .style("pointer-events", "none");

// eventGroups.append("text")
//   .text(d => d.event)
//   .attr("text-anchor", (d, i) => i % 2 === 0 ? "end" : "start")
//   .attr("dx", (d, i) => i % 2 === 0 ? -15 : 15)
//   .attr("dy", 10)
//   .style("font-size", "10px")
//   .style("pointer-events", "none")
//   .style("max-width", "150px")
//   .each(function(d) {
//     const text = d3.select(this);
//     const words = d.event.split(' ');
//     text.text('');

//     let line = [];
//     let lineNumber = 0;
//     const lineHeight = 1.1;
//     const dy = 10;

//     words.forEach((word, i) => {
//       line.push(word);
//       text.text(line.join(' '));
//       if (text.node().getComputedTextLength() > 150 && line.length > 1) {
//         line.pop();
//         text.text(line.join(' '));
//         const tspan = text.append('tspan')
//           .attr('x', 0)
//           .attr('dx', (d, idx) => idx % 2 === 0 ? -15 : 15)
//           .attr('dy', lineNumber === 0 ? dy : lineHeight + 'em')
//           .text(line.join(' '));
//         line = [word];
//         lineNumber++;
//       }
//     });
//     text.text('');
//     text.append('tspan')
//       .attr('x', 0)
//       .attr('dx', (d, idx) => idx % 2 === 0 ? -15 : 15)
//       .attr('dy', dy)
//       .text(line.join(' '));
//   });

// Add title
svg.append("text")
  .attr("x", width / 2)
  .attr("y", -30)
  .attr("text-anchor", "middle")
  .style("font-size", "18px")
  .style("font-weight", "bold")
  .style("fill", getTextColor())
  .text("Interactive Timeline of Christian History");

// Legend (horizontal format in three rows)
const categories = [...new Set(events.map(d => d.category))];
const legend = svg.append("g")
  .attr("transform", `translate(0, ${height + 20})`);

const itemsPerRow = Math.ceil(categories.length / 3);
let xOffset = 0;
let currentRow = 0;

categories.forEach((category, i) => {
  // Start new row after itemsPerRow items
  if (i > 0 && i % itemsPerRow === 0) {
    xOffset = 0;
    currentRow++;
  }

  const legendItem = legend.append("g")
    .attr("transform", `translate(${xOffset}, ${currentRow * 20})`);

  legendItem.append("circle")
    .attr("r", 6)
    .attr("fill", colorScale(category));

  const text = legendItem.append("text")
    .attr("x", 15)
    .attr("y", 5)
    .style("font-size", "10px")
    .style("fill", getTextColor())
    .text(category);

  // Calculate width for next item (text width + circle + spacing)
  const textWidth = text.node().getComputedTextLength();
  xOffset += textWidth + 30;
});
</script>

## Major Periods in Christian History

### Early Christianity (30-313 AD)

The foundational period of Christianity, spanning from Jesus's ministry through nearly three centuries of growth under Roman persecution. This era witnessed the transformation of a small Jewish sect into a widespread religious movement throughout the Mediterranean world.

**Key Events:**

**30 AD - The Crucifixion and Resurrection of Jesus Christ**
- Jesus of Nazareth was crucified under Pontius Pilate during the reign of Emperor Tiberius[^1]
- Christians believe Jesus rose from the dead three days later, which became the central claim of Christian faith
- The resurrection event is recorded in all four Gospels (Matthew 28, Mark 16, Luke 24, John 20-21)
- This event marks the foundation of Christianity and is commemorated annually at Easter

**50 AD - The Council of Jerusalem**
- The first major church council addressed whether Gentile converts must follow Jewish ceremonial law[^2]
- The apostles Peter, James, and Paul played key roles in this decision (Acts 15:1-29)
- The council determined that Gentiles need not be circumcised or follow all Mosaic dietary laws
- This decision opened Christianity to non-Jewish populations and shaped its universal character

**64 AD - Persecution under Emperor Nero**
- Following the Great Fire of Rome, Nero blamed Christians and initiated systematic persecution[^3]
- The Roman historian Tacitus records that Christians were "covered with the skins of beasts" and thrown to dogs, or crucified and burned alive (Annals 15.44)
- Tradition holds that both Peter and Paul were martyred during this persecution
- This began a pattern of intermittent Roman persecution lasting until 313 AD

**Characteristics:**
- **Rapid expansion**: Christianity spread from Jerusalem to Rome, Alexandria, Antioch, and beyond through missionary activity and trade routes
- **Doctrinal formation**: Early Christians defined core beliefs about Jesus's divinity, salvation, and the Trinity
- **Church structure**: Development of ecclesiastical hierarchy including bishops (overseers), presbyters (elders), and deacons
- **Scripture compilation**: The New Testament books were written, circulated, and gradually recognized as authoritative scripture
- **Martyrdom**: Persecution strengthened Christian resolve, with martyrs like Polycarp, Perpetua, and Felicity becoming venerated figures[^4]
- **Early heresies**: The church confronted alternative teachings like Gnosticism, Marcionism, and Montanism

### Imperial Christianity (313-476 AD)

A transformative period when Christianity transitioned from a persecuted minority faith to the official religion of the Roman Empire. This era saw the establishment of orthodox Christian theology through ecumenical councils and the beginning of Christian political power.

**Key Events:**

**313 AD - The Edict of Milan**
- Emperors Constantine and Licinius issued this decree granting religious tolerance throughout the empire[^5]
- Christians gained legal protection, the right to worship openly, and restitution for confiscated property
- Constantine's vision of a cross before the Battle of Milvian Bridge (312 AD) reportedly prompted his favor toward Christianity
- This marked the end of systematic Roman persecution

**325 AD - The Council of Nicaea**
- The first ecumenical council, convened by Constantine to address the Arian controversy[^6]
- Arius taught that Jesus was a created being, subordinate to God the Father
- The council affirmed Jesus's full divinity, declaring him "consubstantial" (homoousios) with the Father
- Produced the Nicene Creed, still recited in churches today: "We believe in one Lord Jesus Christ... true God from true God"
- Established Easter's date calculation and began standardizing Christian practice empire-wide

**380 AD - Christianity Becomes Roman State Religion**
- Emperor Theodosius I issued the Edict of Thessalonica (Cunctos populos) making Nicene Christianity the official state religion[^7]
- The edict declared: "We shall believe in the single Deity of the Father, Son, and Holy Spirit"
- Pagan temples were subsequently closed, and their endowments transferred to churches
- Non-Nicene Christian groups (like Arians) were declared heretics

**451 AD - The Council of Chalcedon**
- The fourth ecumenical council addressed how Christ's divine and human natures relate[^8]
- Rejected both Nestorianism (which divided Christ's natures) and Eutychianism (which merged them)
- Defined the doctrine of the hypostatic union: Christ has two natures united in one person
- This council's decisions led to the separation of Oriental Orthodox churches (Armenian, Coptic, Ethiopian, etc.)

**Characteristics:**
- **Christological debates**: Intense theological discussions about Christ's nature dominated this period
- **Church-state alliance**: Christianity became intertwined with Roman political power and identity
- **Architectural development**: Construction of grand basilicas like Old St. Peter's Basilica and the Church of the Holy Sepulchre
- **Monasticism**: The desert fathers (Anthony, Pachomius) pioneered monastic life; Benedict later established his Rule[^9]
- **Patristic theology**: Great theologians emerged: Augustine of Hippo, Ambrose, Jerome, John Chrysostom, and the Cappadocian Fathers
- **Biblical translation**: Jerome produced the Latin Vulgate translation (382-405 AD)

### Medieval Christianity (476-1517 AD)

The Middle Ages witnessed Christianity's dominance in Europe, the split between Eastern and Western churches, the Crusades, and the height of papal power. This millennium-long period shaped European civilization profoundly.

**Key Events:**

**1054 AD - The Great Schism (East-West Split)**
- The final break between the Roman Catholic Church and Eastern Orthodox Church[^10]
- Theological disputes included the filioque clause (whether the Holy Spirit proceeds from the Father "and the Son"), papal authority, and liturgical practices
- Cardinal Humbert excommunicated Patriarch Michael Cerularius; Cerularius excommunicated the papal legates
- Cultural and political factors (including the sack of Constantinople by Crusaders in 1204) deepened the divide
- These mutual excommunications were only lifted in 1965 during Vatican II

**1095 AD - The First Crusade**
- Pope Urban II called for military expedition to reclaim Jerusalem from Muslim rule[^11]
- Motivated by religious zeal, desire for adventure, and promise of spiritual rewards (plenary indulgences)
- Crusaders captured Jerusalem in 1099, establishing Crusader states in the Holy Land
- Eight major crusades followed over two centuries (1095-1291)
- The Crusades had lasting effects on Christian-Muslim relations, trade, and European culture

**Additional Significant Developments:**
- **800 AD**: Charlemagne crowned Holy Roman Emperor by Pope Leo III, establishing medieval Christendom's political framework
- **1054-1204**: Growing alienation between Eastern and Western Christianity
- **1209-1229**: Albigensian Crusade against Cathar heretics in southern France
- **1378-1417**: Western Schism with multiple competing popes weakened papal authority

**Characteristics:**
- **Papal authority**: The Pope claimed supremacy over secular rulers; Innocent III (1198-1216) represented the apex of papal power
- **Scholasticism**: Integration of Aristotelian philosophy with Christian theology by Thomas Aquinas, Anselm, and others[^12]
- **Universities**: Establishment of educational institutions (Bologna 1088, Paris 1150, Oxford 1167, Cambridge 1209)
- **Religious orders**:
  - Benedictines preserved classical learning
  - Franciscans (founded 1209) emphasized poverty
  - Dominicans (founded 1216) focused on preaching and combating heresy
- **Gothic architecture**: Construction of magnificent cathedrals (Notre-Dame, Chartres, Cologne) expressing heavenly aspirations
- **Popular devotion**: Growth of Marian devotion, saint veneration, pilgrimage, and relic cults
- **Inquisitions**: Established to combat heresy, particularly in Spain (1478)[^13]

### Reformation Era (1517-1648 AD)

The Protestant Reformation shattered Western Christian unity, producing lasting theological, political, and cultural transformations. This period witnessed the rise of multiple Protestant traditions and the Catholic Church's comprehensive response.

**Key Events:**

**1517 AD - Martin Luther's 95 Theses**
- On October 31, 1517, Luther posted his theses on the Wittenberg church door (traditionally)[^14]
- Primarily criticized the sale of indulgences by Johann Tetzel to fund St. Peter's Basilica construction
- Luther's teachings emphasized salvation by faith alone (sola fide), scripture alone (sola scriptura), and grace alone (sola gratia)
- Pope Leo X excommunicated Luther in 1521; Luther publicly burned the papal bull
- At the Diet of Worms (1521), Luther famously declared: "Here I stand, I can do no other"

**1534 AD - English Reformation**
- King Henry VIII broke with Rome after Pope Clement VII refused to annul his marriage to Catherine of Aragon[^15]
- The Act of Supremacy (1534) declared the English monarch the "Supreme Head of the Church of England"
- While initially maintaining Catholic doctrine, the English Church gradually adopted Protestant theology under Edward VI
- Mary I briefly restored Catholicism (1553-1558), but Elizabeth I re-established Protestantism permanently
- The English Reformation had political motivations distinct from continental Protestant theology

**1545-1563 AD - The Council of Trent**
- The Catholic Church's comprehensive response to Protestant challenges[^16]
- Clarified Catholic doctrine on justification, sacraments, scripture and tradition, and purgatory
- Rejected Protestant teachings while implementing internal reforms
- Addressed clerical abuses: required bishops to reside in their dioceses, established seminaries for priest training
- Standardized the Latin Mass (Tridentine Mass) until Vatican II
- Launched the Counter-Reformation and Catholic renewal

**Additional Significant Developments:**
- **1536**: John Calvin publishes *Institutes of the Christian Religion*, establishing Reformed theology[^17]
- **1540**: Ignatius of Loyola founds the Society of Jesus (Jesuits) as a missionary and educational order
- **1555**: Peace of Augsburg allows German princes to choose Catholicism or Lutheranism
- **1618-1648**: Thirty Years' War devastates Europe, ending with the Peace of Westphalia

**Characteristics:**
- **Theological distinctives**: Protestants emphasized biblical authority, justification by faith, priesthood of all believers
- **Vernacular translations**: William Tyndale, Martin Luther, and others translated the Bible into common languages
- **Printing press**: Johannes Gutenberg's invention (1440s) enabled rapid spread of Reformation ideas
- **Religious wars**: Schmalkaldic War, French Wars of Religion, Eighty Years' War
- **Radical Reformation**: Anabaptists, Mennonites, and others advocated believer's baptism and church-state separation
- **Catholic missions**: Jesuits and other orders evangelized Asia, Africa, and the Americas
- **Cultural impact**: Protestant work ethic, literacy promotion, and individualism shaped Western society

### Modern Christianity (1648-Present)

The modern era has witnessed Christianity's global expansion, denominational proliferation, and engagement with secularization, science, and pluralism. Christianity has shifted from European dominance to becoming a truly worldwide faith.

**Key Events:**

**1738 AD - Methodist Movement**
- John Wesley experienced evangelical conversion at a Moravian meeting on Aldersgate Street, London[^18]
- Wesley and his brother Charles pioneered field preaching, reaching common people outside formal church structures
- Methodism emphasized personal holiness, social reform, and "social holiness" (communal spiritual development)
- The movement spread rapidly in Britain and America, particularly among working classes
- By 1784, American Methodism formally organized as a separate denomination
- Wesley's organizational genius created a reproducible model of small groups, itinerant preaching, and lay leadership

**1906 AD - Azusa Street Revival and Pentecostalism**
- Led by African-American preacher William J. Seymour in Los Angeles[^19]
- Emphasized speaking in tongues (glossolalia) as evidence of Holy Spirit baptism
- The revival featured interracial worship unusual for segregated America
- Launched global Pentecostal movement, now comprising over 600 million adherents
- Key denominations include Assemblies of God, Church of God in Christ, and Foursquare Church
- Charismatic renewal later brought Pentecostal practices into Catholic and mainline Protestant churches

**1962-1965 AD - Second Vatican Council (Vatican II)**
- Pope John XXIII called for aggiornamento (updating) of the Catholic Church[^20]
- Allowed vernacular languages in the Mass, replacing mandatory Latin
- Emphasized the church as "People of God," promoting greater lay participation
- *Nostra Aetate* improved relations with non-Christian religions, especially Judaism
- *Unitatis Redintegratio* promoted ecumenical dialogue with other Christian traditions
- *Dignitatis Humanae* affirmed religious freedom as a human right
- The council's reforms sparked both renewal and controversy within Catholicism

**Additional Significant Developments:**
- **1738-1797**: Great Awakening revivals in America (Jonathan Edwards, George Whitefield)
- **1793-1910**: "Great Century" of Protestant missions to Africa, Asia, and the Pacific
- **1910**: Edinburgh Missionary Conference launches modern ecumenical movement
- **1948**: World Council of Churches founded, promoting Christian unity
- **1960s-present**: Liberation theology emerges in Latin America, emphasizing social justice[^21]
- **1979**: Moral Majority mobilizes American evangelical political engagement
- **Late 20th century**: Explosive church growth in Global South (Africa, Asia, Latin America)[^22]

**Characteristics:**
- **Denominational diversity**: Thousands of Christian denominations reflecting theological, cultural, and organizational differences
- **Global shift**: Christianity's center has moved from Europe/North America to Africa, Asia, and Latin America
- **Evangelical movement**: Emphasis on biblical authority, conversion, and evangelism; Billy Graham exemplified mid-20th century evangelicalism
- **Ecumenism**: Dialogues between Catholics, Orthodox, and Protestants; some churches have achieved formal communion
- **Social engagement**: Christian involvement in abolition, civil rights, pro-life movement, environmentalism, poverty relief
- **Secularization challenges**: Declining church attendance in the West; religious "nones" increasing
- **Science and faith**: Ongoing dialogue about evolution, cosmology, bioethics
- **Digital transformation**: Online worship, Christian social media, digital Bible study tools
- **Persecution**: Christianity remains the world's most persecuted religion in many regions[^23]
- **Indigenous Christianity**: Emergence of African Initiated Churches, Asian theological expressions, and contextualized worship

---

## References

[^1]: Ehrman, Bart D. *The New Testament: A Historical Introduction to the Early Christian Writings*. Oxford University Press, 2016.

[^2]: Acts 15:1-29. See also Dunn, James D.G. *Beginning from Jerusalem*. Eerdmans, 2009.

[^3]: Tacitus, *Annals* 15.44. See also Keresztes, Paul. "The Emperor Nero and the Persecution of Christians." *Aufstieg und Niedergang der römischen Welt*, 1979.

[^4]: Moss, Candida R. *The Myth of Persecution: How Early Christians Invented a Story of Martyrdom*. HarperOne, 2013.

[^5]: Lactantius, *De Mortibus Persecutorum* 48. See also Barnes, Timothy D. *Constantine: Dynasty, Religion and Power in the Later Roman Empire*. Wiley-Blackwell, 2011.

[^6]: Ayres, Lewis. *Nicaea and Its Legacy: An Approach to Fourth-Century Trinitarian Theology*. Oxford University Press, 2004.

[^7]: Theodosian Code 16.1.2. See also Rapp, Claudia. *Holy Bishops in Late Antiquity*. University of California Press, 2005.

[^8]: Price, Richard and Mary Whitby, eds. *Chalcedon in Context: Church Councils 400-700*. Liverpool University Press, 2009.

[^9]: Lawrence, C.H. *Medieval Monasticism*. Routledge, 2015.

[^10]: Runciman, Steven. *The Eastern Schism*. Oxford University Press, 1955.

[^11]: Riley-Smith, Jonathan. *The First Crusade and the Idea of Crusading*. University of Pennsylvania Press, 2009.

[^12]: Aquinas, Thomas. *Summa Theologica*. See also Davies, Brian. *The Thought of Thomas Aquinas*. Oxford University Press, 1993.

[^13]: Kamen, Henry. *The Spanish Inquisition: A Historical Revision*. Yale University Press, 1998.

[^14]: Brecht, Martin. *Martin Luther: His Road to Reformation 1483-1521*. Fortress Press, 1985.

[^15]: MacCulloch, Diarmaid. *The Reformation: A History*. Viking, 2003.

[^16]: O'Malley, John W. *Trent: What Happened at the Council*. Harvard University Press, 2013.

[^17]: Calvin, John. *Institutes of the Christian Religion*. Trans. Ford Lewis Battles. Westminster John Knox Press, 1960.

[^18]: Heitzenrater, Richard P. *Wesley and the People Called Methodists*. Abingdon Press, 2013.

[^19]: Anderson, Robert Mapes. *Vision of the Disinherited: The Making of American Pentecostalism*. Hendrickson Publishers, 1979.

[^20]: O'Malley, John W. *What Happened at Vatican II*. Harvard University Press, 2008.

[^21]: Gutiérrez, Gustavo. *A Theology of Liberation*. Orbis Books, 1988.

[^22]: Jenkins, Philip. *The Next Christendom: The Coming of Global Christianity*. Oxford University Press, 2011.

[^23]: Pew Research Center. "Global Restrictions on Religion" reports (annual). Available at pewforum.org.
